window = TK()

from tkinter import
TK interface Tcl/TK

window => 윈도우 창을 화면에 표시
TK() => 반환(윈도우 창)

레이블(Label)
window 위에 test 표현

PhtoImage() => GIF 형식만 지원

경로 => PhotoImage(file = "d://pythonTest//img1.gif")
label1 = Label(window, image = photo)

```python
from tkinter import * # GUI 필수 윈도우, 리눅스, 맥 등에서 동일한 코드로 사용 가능
from tkinter import messagebox

window = Tk()

# 구성하고자하는 내용을 코딩
window.title("처음하는 GUI")
window.geometry("400x100")
window.resizable(width=FALSE, height = FALSE)

label1 = Label(window, text = "Python Easy")
label2 = Label(window, text = "Java Very Easy", fg = "blue")
label3 = Label(window, text = "지각하면 노래를 부르자", width=20, height=5, anchor = SE)

label1.pack()
label2.pack()
label3.pack()



# 함수 선언
def myFunc():
    messagebox.showinfo("귀여운 이미지", "아주 아주 사랑스러운 이미지")

window = Tk()

photo = PhotoImage(file = "/Applications/Python 3.11//img1.gif")
button1 = Button(window, image = photo, command = myFunc)

button1.pack()

window = Tk()

button1 = Button(window, text = "종 료", fg = "red", command = quit)
button1.pack()


# 함수 선언
def myFunc():
    if chk.get() == 0:
        messagebox.showinfo("option", "선택하지 않았군요")
    else:
        messagebox.showinfo("option", "선택하셨군요")

window = Tk()
chk = IntVar()
cb = Checkbutton(window, text = "원하시면 선택을 하세요", variable = chk, command = myFunc)
cb.pack()
```

check button => 사용자가 선택을 하면 1, 사용자가 선택을 하지 않으면 0
결과값이 정수형 타입으로 표시한다 ==> IntVar()
```python
# 함수 선언
def myFunc():
    if var.get() == 1:
        label1.configure(text = "Java")
    elif var.get() == 2:
        label1.configure(text = "C++")
    else:
        label1.configure(text = "Python")

window = Tk()

# 메인 부분
var = IntVar()

rb1 = Radiobutton(window, text = "Java", variable = var, value = 1, command = myFunc)
rb2 = Radiobutton(window, text = "C++", variable = var, value = 2, command = myFunc)
rb3 = Radiobutton(window, text = "Python", variable = var, value = 3, command = myFunc)

label1 = Label(window, text ="가장 쉬운 언어 선택 : ", fg = "skyblue")

rb1.pack()
rb2.pack()
rb3.pack()

label1.pack()

window.mainloop()
```

메뉴
  상위 메뉴
    하위 메뉴
    하위 메뉴

메뉴 자체 = Menu(부모메뉴)
부모 윈도.config(menu = 메뉴 자체)

상위 메뉴 = Menu(메뉴 자체)
메뉴 자체.add_cascade(label="상위메뉴텍스트", menu = 상위메뉴)
상위메뉴.add_command(label="하위메뉴1", command="함수1")
상위메뉴.add_command(label="하위메뉴2", command="함수2")

  
```python

```

__name__ == "__main__"

phto1, photo2, photo3
사용자가 변수를 사용하기 전에 반드시 초기화 해주어야 한다.