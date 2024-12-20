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
        
        A = math.degrees(math.acos(((Line.compute_length(self.edges[1]))**2 + (Line.compute_length(self.edges[2]))**2 - (Line.compute_length(self.edges[0]))**2) / (2 * (Line.compute_length(self.edges[1])) * (Line.compute_length(self.edges[2])))))
        B = math.degrees(math.acos(((Line.compute_length(self.edges[0]))**2 + (Line.compute_length(self.edges[2]))**2 - (Line.compute_length(self.edges[1]))**2) / (2 * (Line.compute_length(self.edges[0])) * (Line.compute_length(self.edges[2])))))
        C = 180 - A - B
        
        self.inner_angles.append(A) 
        self.inner_angles.append(B) 
        self.inner_angles.append(C) 
 
        return self.inner_angles

class Equilateral(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)
    self.is_regular = True

  def compute_area(self):
    return (((Line.compute_length(self.edges[0]))**2)*math.sqrt(3))/4
  
  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_inner_angles(self):
    self.compute_inner_angles = [60]*3
    return self.compute_inner_angles 

class Scalene(Triangle):
  def __init__(self, Bottom_left_corner, Bottom_right_corner, Upper_corner):
    super().__init__(Bottom_left_corner, Bottom_right_corner, Upper_corner)
  
  def compute_perimeter(self):
    return super().compute_perimeter()
  
  def compute_area(self):
        s = self.compute_perimeter() / 2 
        c = Line.compute_length(self.edges[2])
        return math.sqrt(s * (s - Line.compute_length(self.edges[0])) * (s - (Line.compute_length(self.edges[1]))) * (s - (Line.compute_length(self.edges[2]))))
    
  def compute_inner_angles(self):

        
        A = math.degrees(math.acos(((Line.compute_length(self.edges[1]))**2 + (Line.compute_length(self.edges[2]))**2 - (Line.compute_length(self.edges[0]))**2) / (2 * (Line.compute_length(self.edges[1])) * (Line.compute_length(self.edges[2])))))
        B = math.degrees(math.acos(((Line.compute_length(self.edges[0]))**2 + (Line.compute_length(self.edges[2]))**2 - (Line.compute_length(self.edges[1]))**2) / (2 * (Line.compute_length(self.edges[0])) * (Line.compute_length(self.edges[2])))))
        C = 180 - A - B
        
        self.inner_angles.append(A)
        self.inner_angles.append(B)
        self.inner_angles.append(C)
        return self.inner_angles

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
      b = math.atan(Line.compute_length(self.edges[1])/ Line.compute_length(self.edges[0]))
    else:
      b = math.atan(Line.compute_length(self.edges[2])/ Line.compute_length(self.edges[0]))
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
    
print ("\n")

print(f"El cuadrado tiene área de {square.compute_area():.2f} y perímetro de{square.compute_perimeter():.2f}")
print(f"Sus ángulos internos son {square.compute_inner_angles()}")

print ("\n")

p1 = Point(0, 0)
p2 = Point(4, 0)
p3 = Point(2, 4)
p4 = Point(3, 0)
p5 = Point(3, 3)

triangle = Triangle(p1, p2, p3)
isosceles = Isosceles(p1, p2, p3)
equilateral = Equilateral(p1, Point(4, 0), Point(2, math.sqrt(12)))
scalene = Scalene(p1, p4, p5)
trirectangle = Trirectangle(p1, p2, Point(4, 3))

 

print(f"El triangulo inicial tiene un perímetro de {triangle.compute_perimeter():.2f}")

print ("\n")

print(f"El triangulo isosceles tiene un área de {isosceles.compute_area():.2f}, un perímetro de {isosceles.compute_perimeter():.2f}")

print(f"Sus ángulos internos son {isosceles.compute_inner_angles()}")
    
print ("\n")
print(f"El triangulo equilátero tiene un área de {equilateral.compute_area():.2f}, un perímetro de {equilateral.compute_perimeter():.2f}")
print(f"Sus ángulos internos son {equilateral.compute_inner_angles()}")

print ("\n")

print(f"El triangulo escaleno tiene un área de {scalene.compute_area():.2f}, un perímetro de  {scalene.compute_perimeter():.2f}")
print(f"Sus ángulos internos son {scalene.compute_inner_angles()}")

print ("\n")
print(f"El triangulo rectángulo tiene un área de {trirectangle.compute_area():.2f}, un perímetro de {trirectangle.compute_perimeter():.2f}")
print(f"Sus ángulos internos son de {trirectangle.compute_inner_angles()}")

```
## Resultado
```python
El rectángulo tiene área de 12.00 y perímetro de 14.00
Sus ángulos internos son [90, 90, 90, 90]


El cuadrado tiene área de 9.00 y perímetro de12.00
Sus ángulos internos son [90, 90, 90, 90]


El triangulo inicial tiene un perímetro de 12.94


El triangulo isosceles tiene un área de 35.98, un perímetro de 12.94
Sus ángulos internos son [53.15748233594751, 63.42125883202625, 63.42125883202623]


El triangulo equilátero tiene un área de 6.93, un perímetro de 12.00
Sus ángulos internos son [60, 60, 60]


El triangulo escaleno tiene un área de 4.50, un perímetro de  10.24
Sus ángulos internos son [45.035650716454285, 45.035650716454285, 89.92869856709143]


El triangulo rectángulo tiene un área de 10.00, un perímetro de 12.00
```

## Restaurante modificado 

```python
class MenuItem:
    def __init__(self, name: str, price: float, type: str):
        self._name = name
        self._price = price
        self._type = type

    def get_name(self):
        return self._name

    def set_name(self, name: str):
        self._name = name

    def get_price(self):
        return self._price

    def set_price(self, price: float):
        self._price = price

    def get_type(self):
        return self._type

    def set_type(self, type: str):
        self._type = type

    def total_price(self, quantity) -> float:
        return quantity * self._price

class Appetizer(MenuItem):
    def __init__(self, name: str, price: float, type: str, serving: str):
        super().__init__(name, price, type)
        self._serving = serving

    def get_serving(self):
        return self._serving

    def set_serving(self, serving: str):
        self._serving = serving

class Beverage(MenuItem):
    def __init__(self, name: str, price: float, type: str, size: str):
        super().__init__(name, price, type)
        self._size = size

    def get_size(self):
        return self._size

    def set_size(self, size: str):
        self._size = size

class Dessert(MenuItem):
    def __init__(self, name: str, price: float, type: str, flavor: str):
        super().__init__(name, price, type)
        self._flavor = flavor

    def get_flavor(self):
        return self._flavor

    def set_flavor(self, flavor: str):
        self._flavor = flavor

class MainCourse(MenuItem):
    def __init__(self, name: str, price: float, type: str, protein: str, vegetable: str, starch: str):
        super().__init__(name, price, type)
        self._protein = protein
        self._vegetable = vegetable
        self._starch = starch

    def get_protein(self):
        return self._protein

    def set_protein(self, protein: str):
        self._protein = protein

    def get_vegetable(self):
        return self._vegetable

    def set_vegetable(self, vegetable: str):
        self._vegetable = vegetable

    def get_starch(self):
        return self._starch

    def set_starch(self, starch: str):
        self._starch = starch

class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item: MenuItem, quantity: int = 1):
        self.items.append((item, quantity))

    def calculate_total(self) -> float:
        total = sum(item.total_price(quantity) for item, quantity in self.items)
        return total

    def apply_discount(self, discount: float) -> float:
        total = self.calculate_total()
        return total * (1 - discount)

    def display_order(self):
        for item, quantity in self.items:
            print(f'{quantity} x {item.get_name()} - {item.total_price(quantity):.2f} (unidad: {item.get_price():.2f})')

        total = self.calculate_total()
        print(f'Total: {total:.2f}')

    def pay(self, medio_pago: "MedioPago"):
        total = self.calculate_total()
        medio_pago.pagar(total)


class MedioPago:
    def __init__(self):
        pass

    def pagar(self, monto):
        pass

class Tarjeta(MedioPago):
    def __init__(self, numero, cvv):
        super().__init__()
        self.numero = numero
        self.cvv = cvv

    def pagar(self, monto):
        print(f"Pagando {monto:.2f} con tarjeta {self.numero[-4:]}")

class Efectivo(MedioPago):
    def __init__(self, monto_entregado):
        super().__init__()
        self.monto_entregado = monto_entregado

    def pagar(self, monto):
        if self.monto_entregado >= monto:
            print(f"Pago realizado en efectivo. Cambio: {self.monto_entregado - monto:.2f}")
        else:
            print(f"Fondos insuficientes. Faltan {monto - self.monto_entregado:.2f} para completar el pago.")

bebida1 = Beverage("agua", 2.75, "sin gas", "250 ml")
entrada1 = Appetizer("Mini empanadas", 13.50, "Carne", "Grupal")
plato1 = MainCourse("Plato del día", 24.00, "corriente", "carne", "ensalada", "arroz")

order = Order()
order.add_item(bebida1, 2)
order.add_item(entrada1, 1)
order.add_item(plato1, 1)

print("Orden antes del descuento:")
order.display_order()

descuento = 0.10
total_con_descuento = order.apply_discount(descuento)
print(f"\nTotal con {descuento * 100}% de descuento: {total_con_descuento:.2f}")

print("\nRealizando el pago:")
tarjeta = Tarjeta("1234567890123456", "123")
efectivo = Efectivo(50.00)
order.pay(tarjeta)  
order.pay(efectivo)
```
##Resultado
```python
Orden antes del descuento:
2 x agua - 5.50 (unidad: 2.75)
1 x Mini empanadas - 13.50 (unidad: 13.50)
1 x Plato del día - 24.00 (unidad: 24.00)
Total: 43.00

Total con 10.0% de descuento: 38.70

Realizando el pago:
Pagando 43.00 con tarjeta 3456
Pago realizado en efectivo. Cambio: 7.00
```

