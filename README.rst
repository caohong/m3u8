m3u8
====

Python `m3u8`_ parser.

Documentation
=============

The basic usage is to create a playlist object from uri, file path or
directly from a string:

::

    import m3u8

    m3u8_obj = m3u8.load('http://videoserver.com/playlist.m3u8')  # this could also be an absolute filename
    print m3u8_obj.segments
    print m3u8_obj.target_duration

    # if you already have the content as string, use

    m3u8_obj = m3u8.loads('#EXTM3U8 ... etc ... ')

Encryption key
--------------

The segments may be encrypted, in this case the ``key`` attribute will
be an object with all the attributes from `#EXT-X-KEY`_:

-  ``method``: ex.: "AES-128"
-  ``uri``: the key uri, ex.: "http://videoserver.com/key.bin"
-  ``iv``: the initialization vector, if available. Otherwise ``None``.

If no ``#EXT-X-KEY`` is found, the ``key`` attribute will be ``None``.

Multiple keys are not supported yet (and has a low priority), follow
`issue 1`_ for updates.

Variant playlists (variable bitrates)
-------------------------------------

A playlist can have a list to other playlist files, this is used to
represent multiple bitrates videos, and it's called `variant streams`_.
See an `example here`_.

::

    variant_m3u8 = m3u8.loads('#EXTM3U8 ... contains a variant stream ...')
    variant_m3u8.is_variant    # in this case will be True

    for playlist in variant_m3u8.playlists:
        playlist.uri
        playlist.stream_info.bandwidth

the playlist object used in the for loop above has a few attributes:

-  ``uri``: the url to the stream
-  ``stream_info``: a ``StreamInfo`` object (actually a namedtuple) with
   all the attributes available to `#EXT-X-STREAM-INF`_

**NOTE: the following attributes are not implemented yet**, follow
`issue 4`_ for updates

-  ``iframe_stream_info``: usually ``None``, unless it's a playlist with
   `I-Frames`_, in this case it's also a namedtuple ``IFrameStreamInfo``
   with all the attribute available to `#EXT-X-I-FRAME-STREAM-INF`_
-  ``alternative_audios``: its an empty list, unless it's a playlist
   with `Alternative audio`_, in this case it's a list with ``Media``
   objects with all the attributes available to `#X-EXT-MEDIA`_
-  ``alternative_videos``: same as ``alternative_audios``

Running Tests
=============

::

    $ ./runtests

Contributing
============

All contribution is welcome, but we will merge a pull request if, and only if, it

-  has tests
-  follows the code conventions

If you plan to implement a new feature or something that will take more
than a few minutes, please open an issue to make sure we don't work on
the same thing.

.. _m3u8: http://tools.ietf.org/html/draft-pantos-http-live-streaming-09
.. _#EXT-X-KEY: http://tools.ietf.org/html/draft-pantos-http-live-streaming-07#section-3.3.4
.. _issue 1: https://github.com/globocom/m3u8/issues/1
.. _variant streams: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-6.2.4
.. _example here: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-8.5
.. _#EXT-X-STREAM-INF: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-3.4.10
.. _issue 4: https://github.com/globocom/m3u8/issues/4
.. _I-Frames: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-3.4.13
.. _#EXT-X-I-FRAME-STREAM-INF: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-3.4.13
.. _Alternative audio: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-8.7
.. _#X-EXT-MEDIA: http://tools.ietf.org/html/draft-pantos-http-live-streaming-08#section-3.4.9
