# Steps to install pygame for python3 on Macbook

1. Assume python3 and pip are both installed at this point.
2. Install Mercurial (a version control system)
    
    `brew install mercurial`

3. Install git 
    
    `brew install git`

4. install all the dependencies of pygame

    `brew install sdl sdl_image sdl_mixer sdl_ttf smpeg portmidi`
    
5. use pip to install pygame

    `pip install hg+http://bitbucket.org/pygame/pygame`
    
6. Verify pygame is installed and working

    ```
    >>> import pygame
    >>> pygame.Color(255,0,0)
    (255,0,0,255)
    ```
