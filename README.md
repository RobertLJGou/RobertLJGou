- š Hi, Iām @RobertLJGou
- š Iām interested in ...
- š± Iām currently learning ...
- šļø Iām looking to collaborate on ...
- š« How to reach me ...

<!---
RobertLJGou/RobertLJGou is a āØ special āØ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
# -*- coding: utf-8 -*-
"""
Spyder Editor

This is a temporary script file.
"""

from tkinter import *
import random
import time
class Ball:
  def __init__(self, canvas, paddle, color):
    self.canvas = canvas
    self.paddle = paddle
    self.id = canvas.create_oval(60, 60, 180, 180, fill=color)
    self.canvas.move(self.id, 245, 100)
    startx = [-3, -2, -1, 1, 2, 3]
    random.shuffle(startx)
    self.x = startx[0]
    self.y = -3
    self.canvas_height = self.canvas.winfo_height()
    self.canvas_width = self.canvas.winfo_width()
    self.hit_bottom = False
  def draw(self):
    self.canvas.move(self.id, self.x, self.y)
    pos = self.canvas.coords(self.id)#top-left bottom-right
    if (pos[1] <= 0 or self.hit_paddle(pos) == True):
      self.y = -self.y
    if (pos[0] <= 0 or pos[2] >= self.canvas_width):
      self.x = -self.x
    if (pos[3] >= self.canvas_height):
      self.hit_bottom = True
  def hit_paddle(self, pos):
    paddle_pos = self.canvas.coords(self.paddle.id)
    if (pos[2] >= paddle_pos[0] and pos[0] <= paddle_pos[2]):
      if (pos[3] >= paddle_pos[1] and pos[3] <= paddle_pos[3]):
        return True
    return False
class Paddle:
  def __init__(self, canvas, color):
    self.canvas = canvas
    self.id = canvas.create_rectangle(0, 0, 100, 10, fill = color)
    self.x = 0
    self.canvas.move(self.id, 200, 300)
    self.canvas_width = self.canvas.winfo_width()
    self.canvas.bind_all("<Key-Left>", self.turn_left)
    self.canvas.bind_all("<Key-Right>", self.turn_right)
  def draw(self):
    pos = self.canvas.coords(self.id)
    if (pos[0] + self.x >= 0 and pos[2] + self.x <= self.canvas_width):
      self.canvas.move(self.id, self.x, 0)
    #self.x = 0
  def turn_left(self, event):
    self.x = -4
  def turn_right(self, event):
    self.x = 4
tk = Tk()
tk.title("ē½ä¼Æē¹ēå°ęøøę")
tk.resizable(0, 0)#not resizable
tk.wm_attributes("-topmost", 1)#at top
canvas = Canvas(tk, width = 500, height = 500, bd = 0, highlightthickness = 0)
canvas.pack()
tk.update()#init
paddle = Paddle(canvas, 'blue')
ball = Ball(canvas, paddle, 'red')
while 1:
  if (ball.hit_bottom == False):
    ball.draw()
    paddle.draw()
  tk.update_idletasks()
  tk.update()
  time.sleep(0.01)
