# snakegame

import turtle
import time
import random

# score
score = 0
high_score = 0
delay=0.1

#set up the screen
win = turtle.Screen()
win.bgcolor("white")
win.setup(width=400,height=600)
win.tracer(0)


#Snake Head
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("black")
head.penup()
head.goto(0, 100)
head.direction = "stop"

# Snake food
food = turtle.Turtle()
food.speed(0)
food.shape("circle")
food.color("red")
food.penup()
food.goto(0, 0)

# inizializza score
pen = turtle.Turtle()
pen.speed(0)
pen.shape("square")
pen.color("black")
pen.penup()
pen.hideturtle()
pen.goto(0, 220)
pen.write("Score: 0 High Score: {}".format(high_score), align="center", font=("Arial", 9, "normal"))

# ogni segmento è una tartaruga
segments = []

def move():
  global score
  global high_score
  global segments
  # move the end segment in reverse order
  for index in range(len(segments)-1, 0, -1):
    x = segments[index-1].xcor()
    y = segments[index-1].ycor()
    segments[index].goto(x, y)      
    segments[index].showturtle()
    
    # Move segment 0 to where the head is
  if len(segments) > 0:        
    x = head.xcor()
    y = head.ycor()
    segments[0].goto(x, y)
    segments[0].showturtle()


  if head.direction == "up":
    y = head.ycor() #y coordinate of the turtle
    head.sety(y + 20)
 
  if head.direction == "down":
    y = head.ycor() #y coordinate of the turtle
    head.sety(y - 20)
 
  if head.direction == "right":
    x = head.xcor() #y coordinate of the turtle
    head.setx(x + 20)
 
  if head.direction == "left":
    x = head.xcor() #y coordinate of the turtle
    head.setx(x - 20)
    
  # Check for head collision
  for segment in segments:
    if segment.distance(head) < 20:
      time.sleep(1)
      head.goto(0, 0)
      head.direction = "stop"
          
      # Hide the segments
      for segment in segments:
        segment.goto(1000, 1000)
      segments = []

  if head.distance(food) <15:
    # move the food to a random position on screen
    x = random.randint(-200, 200)
    y = random.randint(-200, 200)
    food.goto(x, y)
    # Increase the score
    score = score + 10

    if score > high_score:
        high_score = score      

    new_segment = turtle.Turtle()
    new_segment.hideturtle()
    new_segment.penup()
    new_segment.speed(0)
    new_segment.shape("square")
    new_segment.color("grey")      
    segments.append(new_segment)

  # Check for collision
  if head.xcor() > 290 or head.xcor() < -290 or head.ycor() > 290 or head.ycor() < -290:
    time.sleep(1)
    head.goto(0, 0)
    head.direction = "stop"
    # reset score
    score = 0

  # score
  pen.clear()
  pen.write("score: {} High Score: {}".format(score, high_score), align="center", font=("Courier", 14, "normal"))
    

def go_up():
  if head.direction != "down":
    head.direction = "up"

def go_down():
  if head.direction != "up":
    head.direction = "down"
 
def go_right():
  if head.direction != "left":
    head.direction = "right"
 
def go_left():
  if head.direction != "right":
    head.direction = "left"

# keyboard bindings
win.listen()
win.onkey(go_up, "w")
win.onkey(go_down, "s")
win.onkey(go_right, "d")
win.onkey(go_left, "a")

# Main game loop
while True:  
  win.update()
  move()
  time.sleep(delay)
