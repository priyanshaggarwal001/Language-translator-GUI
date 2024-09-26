#importing modules
from tkinter import *
from translate import Translator
from tkinter import ttk,messagebox,Text
from gtts import gTTS
from playsound import playsound
import os
import mysql.connector
from mysql.connector import Error

# initializing window
window = Tk()
window.title("Language Translation Project")
window.minsize(800,200)
window.maxsize(800,200)

#Source language combo box
label1=ttk.Label(window, text = "Source Language:",font = ("Times New Roman", 10))
label1.grid(row = 0, column = 0,padx = 10, pady = 25)
combo1=ttk.Combobox(window,state='readonly')
combo1['values'] = ("English","spanish","German","French","Italian")
combo1.grid(row = 0,column = 1)
combo1.current(0)

#Destination language combo box
label2=ttk.Label(window, text = "Translated Language:",font = ("Times New Roman", 10))
label2.grid(row = 0,column = 4, padx = 10, pady = 25)
combo2=ttk.Combobox(window,state='readonly')
combo2['values'] = ("English","spanish","German","French","Italian")
combo2.grid(row = 0,column = 5)
combo2.current(0)

#source/Dest. input language box
text1=Text(window, width = 20, height = 5,wrap = WORD, padx = 5, pady = 10)
text1.grid(row = 15,column = 1 )
text2=Text(window, width = 25, height = 5,wrap = WORD, padx = 5, pady = 10)
text2.grid(row = 15,column = 5)

def btnctrl():
   try:
       source_lan=str(combo1.get())
       dest_lan=str(combo2.get())
       input_text=text1.get("1.0","end-1c")
      
       translator= Translator(from_lang=source_lan[:3],to_lang=dest_lan[:3])
       translation = translator.translate(input_text)
       print(translation)
       text2.delete(1.0,"end")
       output_text=text2.insert(1.0,str(translation))
       output_text=text2.get("1.0","end-1c")
          #storing in database
       try:
          connection = mysql.connector.connect(host='localhost',
                                         user='root',
                                         password='Admin@1234')
          if connection.is_connected():
             db_Info = connection.get_server_info()
             print("Connected to MySQL Server version ", db_Info)
             cursor = connection.cursor()
             cursor.execute("create database if not exists translated")
             cursor.execute("use translated")
             cursor.execute("create table if not exists translated_data(source_lan longtext,dest_lan longtext,source_data longtext,dest_data longtext)")
             cursor.execute("insert into translated_data(source_lan,dest_lan,source_data,dest_data) values('"+source_lan+"','"+dest_lan+"','"+input_text+"','"+output_text+"');")
             cursor.execute("commit;")
       except Exception as e:
             print("Error while connecting to MySQL" + str(e))
       finally:
             if connection.is_connected():
                 cursor.close()
                 connection.close()
                 print("MySQL connection is closed")
   except Exception as e:
      messagebox.showerror("try again")
      print(e)
      
def btnctrl1():
   try:
       source_lan=str(combo1.get())
       dest_lan=str(combo2.get())
       input_text=text1.get("1.0","end-1c")
       translator= Translator(from_lang=source_lan[:3],to_lang=dest_lan[:3])
       translation = translator.translate(input_text)
       text2.delete(1.0,"end")
       output_text=text2.insert(1.0,str(translation))
       output_text=text2.get("1.0","end-1c")
       if dest_lan[:2].lower()=="ge":
          langg="de"
          myobj=gTTS(text=output_text,lang=langg,slow=False)
       elif dest_lan[:2].lower()=="sp":
          langgg="es-ES"
          myobj=gTTS(text=output_text,lang=langgg,slow=False)
       else:
          myobj=gTTS(text=output_text,lang=dest_lan[:2].lower(),slow=False)
       myobj.save("test.mp3")
       playsound("test.mp3",False)
       os.remove("test.mp3")

       try:
          connection = mysql.connector.connect(host='localhost',
                                         user='root',
                                         password='Admin@1234')
          if connection.is_connected():
             db_Info = connection.get_server_info()
             print("Connected to MySQL Server version ", db_Info)
             cursor = connection.cursor()
             cursor.execute("create database if not exists translated")
             cursor.execute("use translated")
             cursor.execute("create table if not exists translated_data(source_lan longtext,dest_lan longtext,source_data longtext,dest_data longtext)")
             cursor.execute("insert into translated_data(source_lan,dest_lan,source_data,dest_data) values('"+source_lan+"','"+dest_lan+"','"+input_text+"','"+output_text+"');")
             cursor.execute("commit;")
       except Exception as e:
         print("Error while connecting to MySQL" + str(e))
       finally:
             if connection.is_connected():
                 cursor.close()
                 connection.close()
                 print("MySQL connection is closed")   
   except Exception as e:
      messagebox.showerror("try again")
      print(e)
      
#Button
Text_Convert = ttk.Button(window, text ="Translate", command = btnctrl)
Text_Convert.grid(row = 84,column = 2)
Text_N_Audio_Convert = ttk.Button(window, text ="Translate & Listen", command = btnctrl1)
Text_N_Audio_Convert.grid(row = 84,column = 4)
