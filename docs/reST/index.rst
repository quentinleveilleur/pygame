Pygame Front Page
=================

.. toctree::
   :maxdepth: 2
   :glob:
   :hidden:

   ref/*
   tut/*
   tut/en/**/*
   tut/ko/**/*
   c_api
   filepaths
   logos

Quick start
-----------

Welcome to pygame! Once you've got pygame installed (:code:`pip install pygame` or
:code:`pip3 install pygame` for most people), the next question is how to get a game
loop running. Pygame, unlike some other libraries, gives you full control of program
execution. That freedom means it is easy to mess up in your initial steps.

Here is a good example of a basic setup (opens the window, updates the screen, and handles events)--

.. literalinclude:: ref/code_examples/base_script.py

Here is a slightly more fleshed out example, which shows you how to move something
(a circle in this case) around on screen--

.. literalinclude:: ref/code_examples/base_script_example.py

For more in depth reference, check out the :ref:`tutorials-reference-label`
section below, check out a video tutorial (`I'm a fan of this one
<https://www.youtube.com/watch?v=AY9MnQ4x3zk>`_), or reference the API
documentation by module.

Documents
---------

`Readme`_
  Basic information about pygame: what it is, who is involved, and where to find it.

`Install`_
  Steps needed to compile pygame on several platforms.
  Also help on finding and installing prebuilt binaries for your system.

:doc:`filepaths`
  How pygame handles file system paths.

:doc:`Pygame Logos <logos>`
   The logos of Pygame in different resolutions.


`LGPL License`_
  This is the license pygame is distributed under.
  It provides for pygame to be distributed with open source and commercial software.
  Generally, if pygame is not changed, it can be used with any type of program.

.. _tutorials-reference-label:

Tutorials
---------

:doc:`Introduction to Pygame <tut/PygameIntro>`
  An introduction to the basics of pygame.
  This is written for users of Python and appeared in volume two of the Py magazine.

:doc:`Import and Initialize <tut/ImportInit>`
  The beginning steps on importing and initializing pygame.
  The pygame package is made of several modules.
  Some modules are not included on all platforms.

:doc:`How do I move an Image? <tut/MoveIt>`
  A basic tutorial that covers the concepts behind 2D computer animation.
  Information about drawing and clearing objects to make them appear animated.

:doc:`Chimp Tutorial, Line by Line <tut/ChimpLineByLine>`
  The pygame examples include a simple program with an interactive fist and a chimpanzee.
  This was inspired by the annoying flash banner of the early 2000s.
  This tutorial examines every line of code used in the example.

:doc:`Sprite Module Introduction <tut/SpriteIntro>`
  Pygame includes a higher level sprite module to help organize games.
  The sprite module includes several classes that help manage details found in almost all games types.
  The Sprite classes are a bit more advanced than the regular pygame modules,
  and need more understanding to be properly used.

:doc:`Surfarray Introduction <tut/SurfarrayIntro>`
  Pygame used the NumPy python module to allow efficient per pixel effects on images.
  Using the surface arrays is an advanced feature that allows custom effects and filters.
  This also examines some of the simple effects from the pygame example, arraydemo.py.

:doc:`Camera Module Introduction <tut/CameraIntro>`
  Pygame, as of 1.9, has a camera module that allows you to capture images,
  watch live streams, and do some basic computer vision.
  This tutorial covers those use cases.

:doc:`Newbie Guide <tut/newbieguide>`
  A list of thirteen helpful tips for people to get comfortable using pygame.

:doc:`Making Games Tutorial <tut/MakeGames>`
  A large tutorial that covers the bigger topics needed to create an entire game.

:doc:`Display Modes <tut/DisplayModes>`
  Getting a display surface for the screen.

:doc:`한국어 튜토리얼 (Korean Tutorial) <tut/ko/빨간블록 검은블록/개요>`
  빨간블록 검은블록


Reference
---------

:ref:`genindex`
  A list of all functions, classes, and methods in the pygame package.

:doc:`ref/bufferproxy`
  An array protocol view of surface pixels

:doc:`ref/color`
  Color representation.

:doc:`ref/cursors`
  Loading and compiling cursor images.

:doc:`ref/display`
  Configure the display surface.

:doc:`ref/draw`
  Drawing simple shapes like lines and ellipses to surfaces.

:doc:`ref/event`
  Manage the incoming events from various input devices and the windowing platform.

:doc:`ref/examples`
  Various programs demonstrating the use of individual pygame modules.

:doc:`ref/font`
  Loading and rendering TrueType fonts.

:doc:`ref/freetype`
  Enhanced pygame module for loading and rendering font faces.

:doc:`ref/gfxdraw`
  Anti-aliasing draw functions.

:doc:`ref/image`
  Loading, saving, and transferring of surfaces.

:doc:`ref/joystick`
  Manage the joystick devices.

:doc:`ref/key`
  Manage the keyboard device.

:doc:`ref/locals`
  Pygame constants.

:doc:`ref/mixer`
  Load and play sounds

:doc:`ref/mouse`
  Manage the mouse device and display.

:doc:`ref/music`
  Play streaming music tracks.

:doc:`ref/pygame`
  Top level functions to manage pygame.

:doc:`ref/pixelarray`
  Manipulate image pixel data.

:doc:`ref/rect`
  Flexible container for a rectangle.

:doc:`ref/scrap`
  Native clipboard access.

:doc:`ref/sndarray`
  Manipulate sound sample data.

:doc:`ref/sprite`
  Higher level objects to represent game images.

:doc:`ref/surface`
  Objects for images and the screen.

:doc:`ref/surfarray`
  Manipulate image pixel data.

:doc:`ref/tests`
  Test pygame.

:doc:`ref/time`
  Manage timing and framerate.

:doc:`ref/transform`
  Resize and move images.

:doc:`pygame C API <c_api>`
  The C api shared amongst pygame extension modules.

:ref:`search`
  Search pygame documents by keyword.

.. _Readme: ../wiki/about

.. _Install: ../wiki/GettingStarted#Pygame%20Installation

.. _LGPL License: LGPL.txt

import pygame
import random

# Initialisation de Pygame
pygame.init()

# Définir les dimensions de la fenêtre
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Jeu de tir avec pistolet rouge")

# Définir les couleurs
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Charger les images
gun_image = pygame.image.load('gun_red.png')  # Assurez-vous d'avoir l'image du pistolet rouge
target_image = pygame.image.load('https://img.freepik.com/photos-premium/pistolet-rouge-au-design-futuriste-fond-sombre-ia-generative_7023-106511.jpg')  # Assurez-vous d'avoir l'image de la cible

# Paramètres du jeu
gun_x, gun_y = 50, screen_height // 2  # Position initiale du pistolet
bullet_width, bullet_height = 10, 5
bullet_speed = 10
bullets = []

# Classe pour la cible
class Target(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = target_image
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(600, 780)
        self.rect.y = random.randint(50, 550)
        self.speed = random.randint(1, 3)
        
    def update(self):
        self.rect.x -= self.speed
        if self.rect.x < 0:
            self.rect.x = random.randint(600, 780)
            self.rect.y = random.randint(50, 550)

# Groupe de cibles
targets = pygame.sprite.Group()
for _ in range(5):
    target = Target()
    targets.add(target)

# Boucle principale du jeu
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(WHITE)

    # Gestion des événements
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Déplacer le pistolet (haut/bas)
    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP] and gun_y > 0:
        gun_y -= 5
    if keys[pygame.K_DOWN] and gun_y < screen_height - gun_image.get_height():
        gun_y += 5

    # Tirer une balle
    if keys[pygame.K_SPACE]:
        bullet_x = gun_x + gun_image.get_width()
        bullet_y = gun_y + gun_image.get_height() // 2
        bullets.append([bullet_x, bullet_y])

    # Mouvements des balles
    for bullet in bullets[:]:
        bullet[0] += bullet_speed
        if bullet[0] > screen_width:
            bullets.remove(bullet)
        else:
            pygame.draw.rect(screen, RED, pygame.Rect(bullet[0], bullet[1], bullet_width, bullet_height))

    # Mise à jour des cibles
    targets.update()
    
    # Vérification des collisions
    for target in targets:
        for bullet in bullets[:]:
            if target.rect.colliderect(pygame.Rect(bullet[0], bullet[1], bullet_width, bullet_height)):
                targets.remove(target)
                bullets.remove(bullet)
                # Ajouter une nouvelle cible après la destruction
                new_target = Target()
                targets.add(new_target)

    # Dessiner le pistolet
    screen.blit(gun_image, (gun_x, gun_y))

    # Dessiner les cibles
    targets.draw(screen)

    # Mettre à jour l'affichage
    pygame.display.update()

    # Limiter les FPS
    clock.tick(60)

pygame.quit()


