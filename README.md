# Simple-Calculator

from tkinter import *
from math import *

num1 = num2 = operator = None

def fact(num):
    if num==0 or num==1:
        return 1
    else:
        return num* fact(num-1)

def get_digit(digit):
    current = result_label['text']
    if digit == '.':
        if '.' in current:
            return  # Prevent adding more than one decimal point
        if current == '':
            current = '0'  # Add leading zero before the decimal point
    new = current + str(digit)
    result_label.config(text=new)

def clear():
    global num1, num2, operator
    result_label.config(text='')
    num1 = num2 = operator = None

def delete_last_digit():
    current = result_label['text']
    if current:
        new = current[:-1]
        result_label.config(text=new)

def get_operator(op):
    global num1, operator
    operator = op
    current_text = result_label['text']
    if op in ('sin', 'cos', 'tan', 'log', 'ln'):
        result_label.config(text=f'{op} ')
    elif op == 'x!':
        if current_text == '':
            result_label.config(text='error')
            return
        try:
            num1 = int(current_text)
            if num1 < 0:
                raise ValueError
        except ValueError:
            result_label.config(text='error')
            return
    else:
        if current_text == '':
            result_label.config(text='error')
            return
        try:
            num1 = float(current_text)
        except ValueError:
            result_label.config(text='error')
            return
        result_label.config(text='')

def get_result():
    global num1, num2, operator
    current_text = result_label['text']
    if operator == 'sin':
        try:
            num = float(current_text[4:])  # Extract number from sin(...)
            result_label.config(text=str(round(sin(radians(num)), 4)))
        except:
            result_label.config(text='error')
        return
    elif operator == 'cos':
        try:
            num = float(current_text[4:])  # Extract number from cos(...)
            result_label.config(text=str(round(cos(radians(num)), 4)))
        except:
            result_label.config(text='error')
        return
    elif operator == 'tan':
        try:
            num = float(current_text[4:])  # Extract number from tan(...)
            result_label.config(text=str(round(tan(radians(num)), 4)))
        except:
            result_label.config(text='error')
        return
    elif operator == 'x!':
        try:
            num = float(current_text[-2:])
            result_label.config(text=f'{num}!')
            result_label.after(1000, lambda: result_label.config(text=str(fact(num))))
        except ValueError:
            result_label.config(text='error')
        return
    elif operator == 'log':
        try:
            num = float(current_text[4:])  # Extract number from log(...)
            result_label.config(text=str(round(log10(num), 4)))
        except:
            result_label.config(text='error')
        return
    elif operator == 'ln':
        try:
            num = float(current_text[3:])  # Extract number from ln(...)
            result_label.config(text=str(round(log(num,e), 4)))
        except:
            result_label.config(text='error')
        return

    if current_text == '':
        result_label.config(text='error')
        return

    try:
        num2 = float(current_text)
    except ValueError:
        result_label.config(text='error')
        return

    if operator == '+':
        result_label.config(text=f'{num1} + {num2}')
        result_label.after(1000, lambda: result_label.config(text=f'{num1 + num2}'))
    elif operator == '-':
        result_label.config(text=f'{num1} - {num2}')
        result_label.after(1000, lambda: result_label.config(text=f'{round((num1 - num2),4)}'))
    elif operator == '*':
        result_label.config(text=f'{num1} x {num2}')
        result_label.after(1000, lambda: result_label.config(text=f'{num1 * num2}'))
    elif operator == '/':
        if num2 == 0:
            result_label.config(text='error')
        else:
            result_label.config(text=f'{num1} / {num2}')
            result_label.after(1000, lambda: result_label.config(text=f'{round((num1 / num2),4)}'))
    else:
        result_label.config(text='error')

root = Tk()

root.title('Calculator')
root.geometry('365x370')
root.resizable(0,-100)
root.config(bg='black')

result_label = Label(root,text='',bg='black',fg='white',
                     anchor='e', wraplength=300)
result_label.grid(row=0,column=0,columnspan =15,pady=(50,25),sticky='w')
result_label.config(font=('verdana',40,'bold'))

#buttons 1st row----------------------------------------------
btn7 = Button(root,text='7',bg='#00a65a',fg='white',
              height=1,width=3,
              command= lambda : get_digit(7))
btn7.grid(row=1,column=0)
btn7.config(font=('verdana',20))

btn8 = Button(root,text='8',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(8))
btn8.grid(row=1,column=1)
btn8.config(font=('verdana',20))

btn9 = Button(root,text='9',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(9))
btn9.grid(row=1,column=2)
btn9.config(font=('verdana',20))

btn_add= Button(root,text='+',bg='#00a65a',fg='black',
                height=1, width=3,
                command= lambda : get_operator('+'))
btn_add.grid(row=1,column=3)
btn_add.config(font=('verdana',20))

btn_del= Button(root,text='del',bg='#00a65a',fg='#a42e2e',
                height=1, width=3,
                command= delete_last_digit)
btn_del.grid(row=1,column=4)
btn_del.config(font=('verdana',20))

btn_sin= Button(root,text='sin',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('sin'))
btn_sin.grid(row=1,column=5)
btn_sin.config(font=('verdana',20))

# 2nd row----------------------------------------------
btn4 = Button(root,text='4',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(4))
btn4.grid(row=2,column=0)
btn4.config(font=('verdana',20))

btn5 = Button(root,text='5',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(5))
btn5.grid(row=2,column=1)
btn5.config(font=('verdana',20))

btn6 = Button(root,text='6',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(6))
btn6.grid(row=2,column=2)
btn6.config(font=('verdana',20))

btn_subtract= Button(root,text='-',bg='#00a65a',fg='black',
                     height=1, width=3,
                     command= lambda : get_operator('-'))
btn_subtract.grid(row=2,column=3)
btn_subtract.config(font=('verdana',20))

btn_fact= Button(root,text='x!',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('x!'))
btn_fact.grid(row=2,column=4)
btn_fact.config(font=('verdana',20))

btn_cos= Button(root,text='cos',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('cos'))
btn_cos.grid(row=2,column=5)
btn_cos.config(font=('verdana',20))

#3rd row--------------------------------------------------
btn1 = Button(root,text='1',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(1))
btn1.grid(row=3,column=0)
btn1.config(font=('verdana',20))

btn2 = Button(root,text='2',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(2))
btn2.grid(row=3,column=1)
btn2.config(font=('verdana',20))

btn3 = Button(root,text='3',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(3))
btn3.grid(row=3,column=2)
btn3.config(font=('verdana',20))

btn_mul= Button(root,text='*',bg='#00a65a',fg='black',
                height=1, width=3,
                command= lambda : get_operator('*'))
btn_mul.grid(row=3,column=3)
btn_mul.config(font=('verdana',20))

btn_pt= Button(root,text='.',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_digit('.'))
btn_pt.grid(row=3,column=4)
btn_pt.config(font=('verdana',20))

btn_tan= Button(root,text='tan',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('tan'))
btn_tan.grid(row=3,column=5)
btn_tan.config(font=('verdana',20))

# 4th row--------------------------------------------
btnc = Button(root,text='AC',bg='#00a65a',fg='blue',
              height=1, width=3,
              command = lambda : clear())
btnc.grid(row=4,column=0)
btnc.config(font=('verdana',20))

btn0 = Button(root,text='0',bg='#00a65a',fg='white',
              height=1, width=3,
              command= lambda : get_digit(0))
btn0.grid(row=4,column=1)
btn0.config(font=('verdana',20))

btn_equal = Button(root,text='=',bg='#00a65a',fg='white',
                   height=1, width=3,
                   command= get_result)
btn_equal.grid(row=4,column=2)
btn_equal.config(font=('verdana',20))

btn_devide= Button(root,text='/',bg='#00a65a',fg='black',
                   height=1, width=3,
                   command= lambda : get_operator('/'))
btn_devide.grid(row=4,column=3)
btn_devide.config(font=('verdana',20))

btn_ln= Button(root,text='ln',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('ln'))
btn_ln.grid(row=4,column=4)
btn_ln.config(font=('verdana',20))

btn_log= Button(root,text='log',bg='#00a65a',fg='white',
                height=1, width=3,
                command= lambda : get_operator('log'))
btn_log.grid(row=4,column=5)
btn_log.config(font=('verdana',20))

root.mainloop()
