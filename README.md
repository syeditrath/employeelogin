# employeelogin
import tkinter as tk
import cx_Oracle
import tkinter.messagebox

cn=cx_Oracle.connect(dsn="xe",user="system",password="tiger")
w=tk.Tk()
w.geometry("300x200")
w.title("Login")

l1=tk.Label(w,text="UserName",font=("Arial",15))
l2=tk.Label(w,text="Password",font=("Arial",15))
e1=tk.Entry(w,width=20,font=("Arial",15))
e2=tk.Entry(w,width=20,font=("Arial",15),show="*")
def login():
    u=e1.get()
    p=e2.get()
    c=cn.cursor()
    c.execute("select * from user_profile where uname=:1 and pwd=:2",(u,p))
    r=c.fetchone()
    if r==None:
        tkinter.messagebox.showinfo("info","invalid username or password")
    else:
        w.destroy()
        w1=tk.Tk()
        w1.title("PayRoll System")
        w1.geometry("300x300")
        def add():
            w2=tk.Tk()
            w2.title("Add Employee")
            w2.geometry("300x300+300+300")
            l1=tk.Label(w2,text="EmployeeNo",font=("Arial",14))
            l2=tk.Label(w2,text="EmployeeName",font=("Arial",14))
            l3=tk.Label(w2,text="EmployeeSalary",font=("Arial",14))

            e1=tk.Entry(w2,width=10,font=("Arial",14))
            e2=tk.Entry(w2,width=10,font=("Arial",14))
            e3=tk.Entry(w2,width=10,font=("Arial",14))
            def save():
                eno=e1.get()
                ename=e2.get()
                s=e3.get()
                c=cn.cursor()
                try:
                    c.execute("insert into emp values(:1,:2,:3)",(eno,ename,s))
                    k=c.rowcount
                    if k>0:
                        tkinter.messagebox.showinfo("info","employee added")
                        cn.commit()
                        e1.delete(0,tk.END)
                        e2.delete(0,tk.END)
                        e3.delete(0,tk.END)
                except:
                    tkinter.messagebox.showerror("error","employeeid exists")
            def close():
                w2.destroy()
            
            b1=tk.Button(w2,text="Save",font=("Arial",14),command=save)
            b2=tk.Button(w2,text="Close",font=("Arial",14),command=close)
            l1.grid(row=1,column=1)
            l2.grid(row=2,column=1)
            l3.grid(row=3,column=1)

            e1.grid(row=1,column=2)
            e2.grid(row=2,column=2)
            e3.grid(row=3,column=2)
            b1.grid(row=4,column=1)
            b2.grid(row=4,column=2)
        def update():
            w3=tk.Tk()
            w3.title("Update Employee")
            w3.geometry("300x300")
            l1=tk.Label(w3,text="EmployeeNo",font=("Arial",14))
            l2=tk.Label(w3,text="Salary",font=("Arial",14))
            e1=tk.Entry(w3,width=10,font=("Arial",14))
            e2=tk.Entry(w3,width=10,font=("Arial",14))
            def save():
                eno=e1.get()
                s=e2.get()
                c=cn.cursor()
                c.execute("update emp set sal=sal+:1 where empno=:2",(s,eno))
                k=c.rowcount
                if k>0:
                    tkinter.messagebox.showinfo("info","salary updated")
                    cn.commit()
                    e1.delete(0,tk.END)
                    e2.delete(0,tk.END)
                else:
                    tkinter.messagebox.showerror("error","invalid empno")
            def close():
                w3.destroy()
            b1=tk.Button(w3,text="Save",font=("Arial",14),command=save)
            b2=tk.Button(w3,text="Close",font=("Arial",14),command=close)
            l1.grid(row=1,column=1)
            l2.grid(row=2,column=1)
            e1.grid(row=1,column=2)
            e2.grid(row=2,column=2)
            b1.grid(row=3,column=1)
            b2.grid(row=3,column=2)
        def delete():
            w4=tk.Tk()
            w4.title("Delete Employee")
            w4.geometry("300x300")
            l1=tk.Label(w4,text="EmployeeNo",font=("Arial",14))
            e1=tk.Entry(w4,width=10,font=("Arial",14))
            def dele():
                eno=e1.get()
                c=cn.cursor()
                c.execute("delete from emp where empno=:1",(eno,))
                k=c.rowcount
                if k>0:
                    tkinter.messagebox.showinfo(title="info",message="employee deleted")
                    cn.commit()
                else:
                    tkinter.messagebox.showerror(title="error",message="invalid empno")
            def close():
                w4.destroy()
            
            b1=tk.Button(w4,text="del",font=("Arial",14),command=dele)
            b2=tk.Button(w4,text="close",font=("Arial",14),command=close)
            l1.grid(row=1,column=1)
            e1.grid(row=1,column=2)
            b1.grid(row=2,column=1)
            b2.grid(row=2,column=2)
        def search():
            w5=tk.Tk()
            w5.title("SearchEmployee")
            l1=tk.Label(w5,text="Employeeno",font=("Arial",14))
            e1=tk.Entry(w5,width=10,font=("Arial",14))
            def sear():
                eno=e1.get()
                c=cn.cursor()
                c.execute("select ename,sal from emp where empno=:1",(eno,))
                row=c.fetchone()
                if row!=None:
                    row=list(map(str,row))
                    str1=" ".join(row)
                    tkinter.messagebox.showinfo(title="info",message=str1)
                else:
                    tkinter.messagebox.showinfo(title="error",message="invalid empno")
            
            def close():
                w5.destroy()
            
            b1=tk.Button(w5,text="search",font=("Arial",14),command=sear)
            b2=tk.Button(w5,text="close",font=("Arial",14),command=close)
            l1.grid(row=1,column=1)
            e1.grid(row=1,column=2)
            b1.grid(row=2,column=1)
            b2.grid(row=2,column=2)


            
        b1=tk.Button(w1,text="Add Employee",font=("Arial",14),command=add)
        b2=tk.Button(w1,text="Update Salary",font=("Arial",14),command=update)
        b3=tk.Button(w1,text="Delete Employee",font=("Arial",14),command=delete)
        b4=tk.Button(w1,text="Search Employee",font=("Arial",14),command=search)
        def close():
            w1.destroy()
        b5=tk.Button(w1,text="Close",font=("Arial",14),command=close)
        b1.pack(fill="both")
        b2.pack(fill="both")
        b3.pack(fill="both")
        b4.pack(fill="both")
        b5.pack(fill="both")

def close():
    w.destroy()


b1=tk.Button(w,text="Login",width=10,command=login)
b2=tk.Button(w,text="Close",width=10,command=close)

l1.grid(row=1,column=1)
l2.grid(row=2,column=1)
e1.grid(row=1,column=2)
e2.grid(row=2,column=2)
b1.grid(row=3,column=1)
b2.grid(row=3,column=2)
