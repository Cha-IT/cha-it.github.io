---
layout: post
title: "Kode for å lage Pong i Pygame"
date: 2023-05-11 12:00:00 +0100
categories: programmering python pygame spill spillutvikling
---

```python
import pygame

pygame.init()

# Denne koden lager skjermen og gir den en bredde og høyde
width = 800
height = 500
screen = pygame.display.set_mode([width, height])

# Her lager vi variabler med fargekoder
colorWhite = (255, 255, 255)
colorBlack = (0, 0, 0)

# Her er variablene til ballen og spillerne
ballXPos = 100
ballYPos = 100
ballXSpeed = 0.1
ballYSpeed = 0.1

player1YPos = 200
player2YPos = 200


# Dette er vår spill loop. Den kjøres hele tiden og oppdaterer skjermen
while True:
  
  # Her henter vi input fra tastaturet og beveger spilleren når riktig knapp blir trykket på
  pygame.event.get()
  keys = pygame.key.get_pressed()
  
  if keys[pygame.K_w] and player1YPos > 0:
    player1YPos -= 0.3
    
  if keys[pygame.K_s] and player1YPos < height - 120:
    player1YPos += 0.3
    
  if keys[pygame.K_UP] and player2YPos > 0:
    player2YPos -= 0.3
    
  if keys[pygame.K_DOWN] and player2YPos < height - 120:
    player2YPos += 0.3
    
  
  # Gjør skjermen svart
  screen.fill(colorBlack)
  
  # Her lager vi to variabel med posisjon og størrelse til de to spillerne, også tegner vi de til skjermen
  player1 = (50, player1YPos, 25, 120)
  pygame.draw.rect(screen, colorWhite, player1)
  
  player2 = (width-100, player2YPos, 25, 120)
  pygame.draw.rect(screen, colorWhite, player2)
  
  # Her beveger vi ballen og tegner den til skjermen
  ballXPos += ballXSpeed
  ballYPos += ballYSpeed
  ball = pygame.Rect(ballXPos, ballYPos, 50, 50)
  pygame.draw.ellipse(screen, colorWhite, ball)
  
  # Dette sjekker om ballen er utenfor skjermen på høyre side, og setter ballen i midten om det er sant
  if ballXPos > width - 50:
    ballXPos = width/2
    ballYPos = height/2
  
   # Dette sjekker om ballen er utenfor skjermen på venstre side, og setter ballen i midten om det er sant
  if ballXPos < 0:
    ballXPos = width/2
    ballYPos = height/2
  
   # Dette sjekker om ballen er under skjermen, og får ballen til å gå oppover om det er sant
  if ballYPos > height - 50:
    ballYSpeed = -abs(ballYSpeed)
  
   # Dette sjekker om ballen er over skjermen, og får ballen til å gå oppover om det er sant
  if ballYPos < 0:
    ballYSpeed = abs(ballYSpeed)
    
  # Disse to if-setningene sjekker om ballen kolliderer med en av spillerne, om dette er sant så bytter ballen rettning
  if ball.colliderect(player1):
    ballXSpeed = abs(ballXSpeed)
    
  if ball.colliderect(player2):
    ballXSpeed = -abs(ballXSpeed)
  
  # Dette tegner alt på skjermen slik at vi kan se det
  pygame.display.flip()

```
