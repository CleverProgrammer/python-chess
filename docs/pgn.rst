PGN parsing and writing
=======================

Game model
----------

Games are represented as a tree of moves. Each `GameNode` can have extra
information such as comments. The root node of a game
(`Game` extends `GameNode`) also holds general information, such as game
headers.

.. autoclass:: chess.pgn.Game
    :members:

    .. py:attribute:: headers

        A `collections.OrderedDict()` of game headers.

    .. py:attribute:: errors

        A list of illegal or ambiguous move errors encountered while parsing
        the game.

.. autoclass:: chess.pgn.GameNode
    :members:

    .. py:attribute:: parent

        The parent node or `None` if this is the root node of the game.

    .. py:attribute:: move

        The move leading to this node or `None` if this is the root node of the
        game.

    .. py:attribute:: nags
        :annotation: = set()

        A set of NAGs as integers. NAGs always go behind a move, so the root
        node of the game can have none.

    .. py:attribute:: comment
        :annotation: = ''

        A comment that goes behind the move leading to this node. Comments
        that occur before any move are assigned to the root node.

    .. py:attribute:: starting_comment
        :annotation: = ''

        A comment for the start of a variation. Only nodes that
        actually start a variation (`starts_variation()`) can have a starting
        comment. The root node can not have a starting comment.

    .. py:attribute:: variations

        A list of child nodes.

Parsing
-------

.. autofunction:: chess.pgn.read_game

.. autofunction:: chess.pgn.scan_headers

.. autofunction:: chess.pgn.scan_offsets

Writing
-------

If you want to export your game game with all headers, comments and variations
you can use:

>>> print(game)
[Event "?"]
[Site "?"]
[Date "????.??.??"]
[Round "?"]
[White "?"]
[Black "?"]
[Result "*"]
<BLANKLINE>
1. e4 e5 { Comment } *

Remember that games in files should be separated with extra blank lines.

>>> print(game, file=handle, end="\n\n")

Use exporter objects if you need more control. Exporter objects are used to
allow extensible formatting of PGN like data.

.. autoclass:: chess.pgn.StringExporter
    :members:

.. autoclass:: chess.pgn.FileExporter
    :members:

NAGs
----

Numeric anotation glyphs describe moves and positions using standardized codes
that are understood by many chess programs. During PGN parsing, annotations
like ``!``, ``?``, ``!!``, etc. are also converted to NAGs.

.. autodata:: chess.pgn.NAG_GOOD_MOVE
.. autodata:: chess.pgn.NAG_MISTAKE
.. autodata:: chess.pgn.NAG_BRILLIANT_MOVE
.. autodata:: chess.pgn.NAG_BLUNDER
.. autodata:: chess.pgn.NAG_SPECULATIVE_MOVE
.. autodata:: chess.pgn.NAG_DUBIOUS_MOVE
