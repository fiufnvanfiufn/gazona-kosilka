# Проект робот-газонокосилка
В этом гите будет продемонстрирован код робота газонокосилки, вмесет с последеющими пояснениями

___

## Принцип работы
Стоит начать с того, как собственно работает робот. Стартует робот с точки, находящейся на краю участка, дальше робот будет поворачиваться налево, ехать пока не заметит дорогу/стену. Вот часть кода для определения наличия травы. Чтобы это реализовать используем библиотеку opencv2.


  import cv2
  cam = cv2.VideoCapture(0)
  while True:
      ret, frame = cam.read()
      gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
      edges = cv2.Canny(gray, 100, 200)
      contours, hierarchy = cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
      for contour in contours:
          area = cv2.contourArea(contour)
          if area > 1000:
              cv2.drawContours(frame, contour, -1, (0, 255, 0), 3)
      cv2.imshow('Grass Detection', frame)

      if cv2.waitKey(1) & 0xFF == ord('q'):
          break
  cam.release()
  cv2.destroyAllWindows()

___

Дальше робот едет по периметру участка, доезжая до своего старта, поворачивает направо и проезжает вперед на свою длину, также проезжает по периметру нескошенной травы до места старта, и так до того момента, пока робот не приедет на центр поля, после он возвращяется на стартовую позицию.

