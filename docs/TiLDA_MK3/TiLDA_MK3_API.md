### GET api-badge.emf.camp/apps.json

    {
        "games": [
            {
                user: "someuser"
                name: "snake",
                description: "Some game with a snake",
                files: {
                    "main.py": "abcdefgh",
                    "images.bmp": "abcdefg"
                }
            },
            {
                user: "someuser2"
                name: "tetris",
                description: "Some game with a snake",
                files: {
                    "main.py": "abcdefgh",
                    "images.bmp": "abcdefg"
                }
            }
        ],
        "emf": [...]
        ...
    }

### GET api-badge.emf.camp/someuser/snake.json

    {
        user: "someuser"
        name: "snake",
        description: "Some game with a snake",
        files: {
            "main.py": "abcdefgh",
            "images.bmp": "abcdefg"
        }
    }