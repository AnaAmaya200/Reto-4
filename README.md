# Reto-4

## 1. Ejercicio en clase

```python

class Shape:
   def __init__(self):
      pass
   def compute_area(self):
      pass
   def compute_perimeter(self):
      pass

class Rectangle(Shape):
  def __init__(self, width, height):
    super().__init__()
    self.width = width
    self.height = height
  def compute_area(self):
     return self.height*self.width

  def compute_perimeter(self):
    return 2*self.height + 2*self.width
  
class Square(Shape):
  def __init__(self, side):
    super().__init__()
    self.side = side

  def compute_area(self):
     return self.side**2

  def compute_perimeter(self):
    return 4*self.side

rect = Rectangle(5, 10)
square = Square(7)


print(f"El Rectángulo tiene un ancho de  {rect.width} y altura de  {rect.height}")
print(f"Su Área es de  {rect.compute_area()}")
print(f"su Perímetro de {rect.compute_perimeter()}")

print(f"El cuadrado tiene lados de {square.side}")
print(f"Su Área es de {square.compute_area()}")
print(f"Su Perímetro es de {square.compute_perimeter()}")
```
resultado

```python
El Rectángulo tiene un ancho de  5 y altura de  10
Su Área es de  50
su Perímetro de 30
El cuadrado tiene lados de 7
Su Área es de 49
Su Perímetro es de 28
```

## 2. Ejercicio en clase


```python
import math

class Point:
  def __init__(self, x=0.0, y=0.0):
    self.x = float(x) 
    self.y = float(y)
  def compute_distance(self, point:"Point"):
    return ((self.x - point.x)**2+(self.y - point.y)**2)**(0.5)

class Line:
  def __init__(self,start: Point,end: Point):
    self.start = start
    self.end = end
  def compute_length(self):
    self.length = (round(Point.compute_distance(self.start, self.end),2))
    return self.length

class Shape:
  def __init__(self):
    self.is_regular = None
    self.vertices = []
    self.edges = []
    self.inner_angles = []
  def compute_area(self):
    pass
  def compute_perimeter(self):
    pass
  def compute_inner_angles(self):
    pass

class Rectangle(Shape):
  def __init__(self, Bottom_left_corner, Upper_right_corner):
    super().__init__()
    self.is_regular = False
    Bottom_right_corner = Point(Upper_right_corner.x,Bottom_left_corner.y)
    Upper_left_corner = Point(Bottom_left_corner.x,Upper_right_corner.y)
    self.vertices.append(Bottom_left_corner)
    self.vertices.append(Upper_left_corner)
    self.vertices.append(Upper_right_corner)
    self.vertices.append(Bottom_right_corner)
    self.edges.append(Line(Bottom_left_corner,Upper_left_corner))
    self.edges.append(Line(Upper_left_corner,Upper_right_corner))
    self.edges.append(Line(Upper_right_corner,Bottom_right_corner))
    self.edges.append(Line(Bottom_right_corner,Bottom_left_corner))
    
  def compute_area(self):
     return Line.compute_length(self.edges[0])*Line.compute_length(self.edges[1])

  def compute_perimeter(self):
    return (2*Line.compute_length(self.edges[0]))+(2*Line.compute_length(self.edges[1]))
  
  def compute_inner_angles(self):
    self.inner_angles = [90]*4
    return self.inner_angles
  
class Square(Rectangle):
  def __init__(self, Bottom_left_corner, Upper_right_corner):
    super().__init__(Bottom_left_corner, Upper_right_corner)
    self.is_regular = True
  def compute_area(self):
    return super().compute_area()
  def compute_inner_angles(self):
    return super().compute_inner_angles()



class Triangle(Shape):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__()
    self. is_regular = False
    self.vertices.append(Bottom_left_corner)
    self.vertices.append(Bottom_right_corner)
    self.vertices.append(Upper_corner)
    self.edges.append(Line(Bottom_left_corner,Bottom_right_corner))
    self.edges.append(Line(Bottom_right_corner,Upper_corner))
    self.edges.append(Line(Upper_corner,Bottom_left_corner))
  def compute_perimeter(self):
    return Line.compute_length(self.edges[0]) + Line.compute_length(self.edges[1]) + Line.compute_length(self.edges[2])
  
class Isosceles(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)

  def compute_area(self):
    h = ((Line.compute_length(self.edges[1]))**2+(Line.compute_length(self.edges[0])**2))**1/2
    return (Line.compute_length(self.edges[0])*h)/2
  
  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_inner_angles(self):
    a = math.acos((2*(Line.compute_length(self.edges[1])**2)-Line.compute_length(self.edges[0]))/2*(Line.compute_length(self.edges[1])**2))
    b = (180-a)/2
    self.inner_angles.append(a)
    self.inner_angles.append(b)
    self.inner_angles.append(b)

class Equilateral(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)
    self.is_regular = True

  def compute_area(self):
    return (((Line.compute_length(self.edges[0]))**2)*math.sqrt(3))/4
  
  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_inner_angles(self):
    return self.compute_inner_angles [60]*3

class Scalene(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)
  
  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_area(self):
    s = (self.compute_perimeter(self))/2
    return math.sqrt(s*(s-Line.compute_length(self.edges[0])*(s-Line.compute_length(self.edges[1])*(s-Line.compute_length(self.edges[2])))))
  
  def compute_inner_angles(self):
    a = math.acos(((Line.compute_length(self.edges[1])**2)+(Line.compute_length(self.edges[2])**2)-(Line.compute_length(self.edges[0])**2))/2*(Line.compute_length(self.edges[1]))*(Line.compute_length(self.edges[2])))
    b = math.acos((-(Line.compute_length(self.edges[1])**2)+(Line.compute_length(self.edges[2])**2)+(Line.compute_length(self.edges[0])**2))/2*(Line.compute_length(self.edges[0]))*(Line.compute_length(self.edges[2])))
    c = math.acos(((Line.compute_length(self.edges[1])**2)-(Line.compute_length(self.edges[2])**2)+(Line.compute_length(self.edges[0])**2))/2*(Line.compute_length(self.edges[0]))*(Line.compute_length(self.edges[1])))
    self.inner_angles.append(a)
    self.inner_angles.append(b)
    self.inner_angles.append(c)

class Trirectangle(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)

  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_area(self):
    if self.vertices[1].y == self.vertices[2].y :
      return (Line.compute_length(self.edges[0])* Line.compute_length(self.edges[1]))/2
    else:
      return (Line.compute_length(self.edges[0])* Line.compute_length(self.edges[2]))/2
    
  def compute_inner_angles(self):
    a = 90
    if self.vertices[1].y == self.vertices[2].y :
      b = math.atan2(Line.compute_length(self.edges[1])/ Line.compute_length(self.edges[0]))
    else:
      b = math.atan2(Line.compute_length(self.edges[2])/ Line.compute_length(self.edges[0]))
    c = a - b
    self.inner_angles.append(a)
    self.inner_angles.append(b)
    self.inner_angles.append(c)

bottom_left_corner_rect = Point(0, 0)
upper_right_corner_rect = Point(4, 3)
    
bottom_left_corner_square = Point(0, 0)
upper_right_corner_square = Point(3, 3)
    

rect = Rectangle(bottom_left_corner_rect, upper_right_corner_rect)
square = Square(bottom_left_corner_square, upper_right_corner_square)

print(f"El rectángulo tiene área de {rect.compute_area():.2f} y perímetro de {rect.compute_perimeter():.2f}")
print(f"Sus ángulos internos son {rect.compute_inner_angles()}")
    
print(f"El cuadrado tiene área de {square.compute_area():.2f} y perímetro de{square.compute_perimeter():.2f}")
print(f"Sus ángulos internos son {square.compute_inner_angles()}")

```
