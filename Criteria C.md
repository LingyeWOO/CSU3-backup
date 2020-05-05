Criteria C: Development
====

### Step 1: Converting UI files to Python files

```.py
(venv) C:\Users\Lingye\PycharmProjects\practice1\Tuan>pyuic5 Mainwindow.ui > Mainwindow2.py

(venv) C:\Users\Lingye\PycharmProjects\practice1\Tuan>pyuic5 login2.ui > login.py

```
In this step, we are utilizing three tools:
- QT designer
- pyuic5
- python3

Through these 3 tools, we are able to convert all UI files to python files that allow us to edit the interface with code directly.

### Step 2: Create "Main" file
```.py
import sys
from PyQt5 import QtCore, QtGui, QtWidgets
from Tuan.Mainwindow2 import homepage
from Tuan.login import login
from PyQt5.QtWidgets import QApplication, QMainWindow, QDialog
```
The main file is the file where we import all other classes from the ui files, therefore operate through creating new classes by inheriting
from other files. The advantage of having a main file is:
- preventing data loss and code piracy
- clear in organization
- clear in both functionalities: create and operate

### Step 3: Develop "Main" file

In the maine file, the code is organized by different windows(In this case, Qdialogs and one mainwindow).
First, I inheritted the two windows I have from their mother file, and changed its class name to make it look easier:

```.py
class mainWindowApp(QMainWindow, homepage):
    def __init__(self, parent=None):
        super(mainWindowApp, self).__init__(parent)
        self.setupUi(self)
        
class LoginApp(login):
    def __init__(self, parent=None):
        super(LoginApp, self).__init__(parent)
        self.setupUi(self)
```

Second, I designed the functionalities of different push buttons to make sure different methods are called when each button is pushed under
different circumstances, for instance:

```.py
self.login.clicked.connect(self.try_login)

    def try_login(self):
        email = self.username_label.text()
        pd = self.passcode.text()
        if email == "2021.tuan.anh@uwcisak.jp":
            self.username.setStyleSheet("border: 2px solid green")
        else:
            self.username.setStyleSheet("border: 2px solid red")

        if pd == "123456":
                self.passcode.setStyleSheet("border: 2px solid green")
        else:
            self.passcode.setStyleSheet("border: 2px solid red")
```

- This snippet of code demonstrates when the "login" button is pushed, the program runs a automatic index check for the "username"
and the "passcode", to see if both of them match with the default setting by our client. If they match, the border of both text boxes turn
green. Otherwise, they turn red.

Thirdly, I made the login page to show before the mainwindow.
```.py
 # To make the login page appear before the mainwindow
        logVar = LoginApp(self)
        logVar.show()
 ```

### Step 4: Mainwindow
For the mainwindow, it is important to develop several different functionalities:
1. There are four buttons:
  - log out
  - exit
  - add
  - edit
  
  And all four of them should have a different functionality as follow:
  
  ```.py
  self.Exit.clicked.connect(self.exitApp)
  self.signout.clicked.connect(self.openlogin)
  self.tableWidget.cellChanged.connect(self.changeDB)
  self.Edit.clicked.connect(self.save)
  self.Add.clicked.connect(self.cancel)
  ```
  
  All of the methods called above are coded as:
  
  ```.py
      def exitApp(self):
        sys.exit(0) # 0 means exit without erros

    # re-opens the login page
    def openlogin(self):
        sys.exit(0)

    # Edit/add a pair
    def changeDB(self):
        item = self.tableWidget.currentItem() #cell clicked
        row = self.tableWidget.currentRow() #row clicked
        col = self.tableWidget.currentColumn() #colum clicked

        #change the colorof the cell clicked
        self.tableWidget.item(row, col).setBackground(QtGui.QColor(100, 100, 150))

        #show the item in the terminal for debugging purposes
        print(item.text())

        self.Edit.setDisabled(False)
        self.Add.setDisabled(False)

    def save(self):
        print("Save to CSV File")

    def cancel(self):
        print("Reload the table")
   ```
    
### Step 5 Finalizing the code

After finalizing the code through the show function, the coding part is ended.
```.py
app = QApplication(sys.argv)  # creating the application with arguments from user
login_show = LoginApp() # setting the main window to the Home UI
main = mainWindowApp()
main.show()
app.exec_()

```
