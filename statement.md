import turtle
import os
import math
import random

# Create the window
window=turtle.Screen()
window.bgcolor('black')
window.title('Dodge')

# Draw the border
border=turtle.Turtle()
border.speed(0)
border.penup()
border.pensize(3)
border.hideturtle()
border.setpos(-50, -100)
border.color('white')
border.pendown()
for side in range(2):
    border.fd(100)
    border.lt(90)
    border.fd(200)
    border.lt(90)

# Create the player
player=turtle.Turtle()
player.speed(0)
player.penup()
player.color('Blue')
#player.shape('triangle')
player.setheading(90)
player.shapesize(1.75)
player.setpos(0, -75)

# Create the lives
lives = 3

life1=turtle.Turtle()
life1.penup()
life1.setpos(-46, 115)
life1.color('Blue')
life1.shapesize(1)
life1.setheading(90)

life2=turtle.Turtle()
life2.penup()
life2.setpos(-32, 115)
life2.color('Blue')
life2.shapesize(1)
life2.setheading(90)

life3=turtle.Turtle()
life3.penup()
life3.setpos(-18, 115)
life3.color('Blue')
life3.shapesize(1)
life3.setheading(90)

# Create the life system
def life():
    if lives is 2:
        life3.hideturtle()

    if lives is 1:
        life2.hideturtle()

    if lives is 0:
        life1.hideturtle()

def hideall():
    player.hideturtle()
    enemy1.hideturtle()
    enemy2.hideturtle()
    bullet1.hideturtle()
    bullet2.hideturtle()

# Define the round system

# Define Movement 
Speed=33.3333333333

def move_left():
    x = player.xcor()
    x -= Speed
    if x < -33:
        x = -33
    player.setx(x)

def move_right():
    x = player.xcor()
    x += Speed
    if x > 33:
        x = 33
    player.setx(x)

# Create the control scheme 
turtle.listen()
turtle.onkey(move_left, 'Left')
turtle.onkey(move_right, 'Right')

# Create the enemies 
enemy1=turtle.Turtle()
enemy1.speed(0)
enemy1.color('red')
enemy1.shapesize(1.75)
enemy1.penup()
enemy1.setpos(-33, 75)
enemy1.setheading(-90)

enemy2=turtle.Turtle()
enemy2.speed(0)
enemy2.color('red')
enemy2.shapesize(1.75)
enemy2.penup()
enemy2.setpos(33, 75)
enemy2.setheading(-90)

# Create enemy weapons
bullet1=turtle.Turtle()
bullet1.penup()
bullet1.setpos(enemy1.xcor(), 75)
bullet1.speed(0)
bullet1.shapesize(0.40)
bullet1.setheading(-90)
bullet1.shape('triangle')
bullet1.color('yellow')

bullet2=turtle.Turtle()
bullet2.penup()
bullet2.setpos(enemy2.xcor(), 75)
bullet2.speed(0)
bullet2.shapesize(0.40)
bullet2.setheading(-90)
bullet2.shape('triangle')
bullet2.color('yellow')

def collision_checking(t1, t2):
    distance = math.sqrt(math.pow(t1.xcor()-t2.xcor(), 2)+math.pow(t1.ycor()-t2.ycor(), 2))
    if distance < 10:
        return True
    else:
        return False

# Create enemy movement 
def enemy_movement():
    def enemy_move():
        enemy_speed=random.randrange(1, 4)

        if enemy_speed is 1:
            enemy1.setpos(-33, 75)
            enemy2.setpos(33, 75)
            bullet1.setx(-33)
            bullet2.setx(33)

        if enemy_speed is 2:
            enemy1.setpos(0, 75)
            enemy2.setpos(-33, 75)
            bullet1.setx(0)
            bullet2.setx(-33)

        if enemy_speed is 3:
            enemy1.setpos(33, 75)
            enemy2.setpos(0, 75)
            bullet1.setx(33)
            bullet2.setx(0)

        if enemy_speed is 4:
            enemy1.setpos(-33, 75)
            enemy2.setpos(0, 75)
            bullet1.setx(-33)
            bullet2.setx(0)

    bullet_original_speed=2.5
    bullet_speed=2.5
    bullet_speed_limit=10

    if bullet_speed > bullet_speed_limit:
        difficulty_level += 1
        enemy_original_speed += 0.5
        enemy_speed = enemy_original_speed
        enemy_speed_limit += 1.5  

    y1 = bullet1.ycor()
    y2 = bullet2.ycor()
    x1 = enemy1.xcor()
    x2 = enemy2.xcor()
    y1 -= bullet_speed
    y2 -= bullet_speed
    if y1 < -80:
        enemy_move()
        x1=enemy1.xcor()
        x2=enemy2.xcor()
        y1 = 75
        y2 = 75 
        #bullet1.setx(x1)
        #bullet2.setx(x2)
        bullet_speed += 0.5
    print(bullet_speed)
    bullet1.sety(y1)
    bullet2.sety(y2)

while True:
    enemy_movement()
    life()

    y2_1=bullet2.ycor()
    #if y2_1 < -75:
        #rounds += 2
        #round_display.write(roundstring, False, align='Left', font=('Arial', 14, 'normal'))

    if collision_checking(bullet1, player):
        enemy1.setpos(-33, 75)
        enemy2.setpos(33, 75)
        player.setpos(0, -75)
        #bullet1.hideturtle()
        #bullet2.hideturtle()
        bullet1.setpos(33, 75)
        bullet2.setpos(-33, 75)
        y = bullet1.ycor()
        lives -= 1
        #if y < -65:
           # bullet1.sety(75)
            #bullet2.sety(75)

    if collision_checking(bullet2, player):
        enemy1.setpos(-33, 75)
        enemy2.setpos(33, 75)
        player.setpos(0, -75)
        #bullet1.hideturtle()
        #bullet2.hideturtle()
        bullet1.setpos(33, 75)
        bullet2.setpos(-33, 75)
        y = bullet1.ycor()
        lives -= 1
        #if y < -65:
           # bullet1.sety(75)
            #bullet2.sety(75)

    if lives is 0:
        hideall()
        break
        
delay=raw_input('Press enter to end process.')
