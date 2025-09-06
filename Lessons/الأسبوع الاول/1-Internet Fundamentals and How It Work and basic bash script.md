# Lesson 1: Internet Fundamentals and How It Works

## 🌐 Language / اللغة

- [🇺🇸 English Version](#english-version)
- [🇪🇬 النسخة العربية](#arabic-version)

---
## English Version

## Lesson Objectives
- Understand how the internet works
- Learn about HTTP Protocol
- Understand DNS
- Get familiar with TCP/IP
- Command Line basics

## 1. What is the Internet?

The Internet is a massive network of interconnected computers around the world. Think of it like a road network, but instead of cars, data moves through it.

### Basic Internet Components:
- **Servers**: Powerful computers that host websites
- **Clients**: User devices (mobile phones, laptops)
- **Protocols**: Rules that govern communication

## 2. How HTTP Requests Work?

When you type www.google.com in your browser, the following happens:

```
1. Browser sends HTTP Request
2. Server receives the request
3. Server sends HTTP Response
4. Browser displays the page
```

### Practical Example - HTTP Request:
```http
GET /search?q=fullstack HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0
Accept: text/html
```

### HTTP Response:
```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<!DOCTYPE html>
<html>...</html>
```

## 3. DNS System - The Internet's Directory

DNS is like the phone book of the internet. It converts names to IP addresses.

### DNS Lookup Process:
1. You type: `facebook.com`
2. DNS translates it to: `157.240.241.35`
3. Browser connects to that address

```javascript
// Simple example in JavaScript
console.log('Want to access: facebook.com');
console.log('DNS translates to: 157.240.241.35');
console.log('Connection established!');
```

## 4. TCP/IP Protocol

TCP/IP is the language that computers use to communicate with each other:

### TCP (Transmission Control Protocol):
- Ensures data arrives complete
- Arranges data in correct order
- Like registered mail

### IP (Internet Protocol):  
- Determines each device's address
- Like a house address

## 5. Practical Example - How a Simple Website Works

```javascript
// Simple Node.js example
const http = require('http');

// Create server
const server = http.createServer((req, res) => {
    // Receive request from client
    console.log('New request:', req.url);
    
    // Send response
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    res.end('<h1>Hello! My first server</h1>');
});

// Run server on port 3000
server.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

## 6. Important HTTP Methods

```javascript
// GET - to fetch data
GET /users          // Get all users

// POST - to add new data  
POST /users         // Add new user

// PUT - to update data
PUT /users/123      // Update user 123

// DELETE - to delete data
DELETE /users/123   // Delete user 123
```

## 7. Important Status Codes

```
200 OK          - Everything is fine
201 Created     - Data created successfully  
400 Bad Request - Invalid request
401 Unauthorized- Not authorized
404 Not Found   - Page not found
500 Server Error- Server error
```

## 8. Practical Exercise

Create a simple server that responds to different requests:

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
    const pathname = url.parse(req.url).pathname;
    
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    
    switch(pathname) {
        case '/':
            res.end('<h1>Home Page</h1>');
            break;
        case '/about':
            res.end('<h1>About Us</h1>');
            break;
        case '/contact':
            res.end('<h1>Contact Us</h1>');
            break;
        default:
            res.writeHead(404);
            res.end('<h1>Page Not Found</h1>');
    }
});

server.listen(3000);
```

## Homework

1. Try the code above and run it
2. Try adding new pages
3. Study Status Codes and memorize them
4. Read about HTTP Headers and their types

## Quick Quiz

### Questions:
1. What is DNS's function?
2. What's the difference between GET and POST?
3. What does Status Code 404 mean?
4. Write an example of a simple HTTP Request

### Answers:
1. DNS converts website names to IP addresses
2. GET fetches data, POST adds new data
3. Page or resource not found
4. GET /users HTTP/1.1, Host: example.com

## 9. Command Line & Bash Basics

### Important Basic Commands:
```bash
# Navigate folders
pwd                 # Where am I now?
ls                  # What files are here?
cd folder_name      # Go to folder
cd ..              # Go back to previous folder

# File management
mkdir new_folder    # Create new folder
touch file.txt      # Create empty file
cp file1.txt file2.txt  # Copy file
mv old.txt new.txt  # Rename or move
rm file.txt         # Delete file
rm -rf folder/      # Delete folder and all contents

# Display file contents
cat file.txt        # Show file content
head file.txt       # First 10 lines
tail file.txt       # Last 10 lines
grep "text" file.txt # Search in file
```

### Example Simple Bash Script:
```bash
#!/bin/bash

# Script to create new Node.js project
echo "Creating new project..."

# Create folder
mkdir my-project
cd my-project

# Create basic files
npm init -y
touch index.js
touch .gitignore

# Add basic content
echo "console.log('Hello new project!');" > index.js
echo "node_modules/" > .gitignore

echo "Project ready!"
```

## 10. Environment Variables

```bash
# Set variable
export DATABASE_URL="mongodb://localhost:27017/myapp"
export PORT=3000

# Use variables in Node.js
# process.env.DATABASE_URL
# process.env.PORT
```

## 11. Complete Example - Server with Bash Management

### server.js
```javascript
const http = require('http');
const port = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
    res.writeHead(200, {'Content-Type': 'application/json'});
    res.end(JSON.stringify({
        message: 'Server is running!',
        port: port,
        time: new Date().toISOString()
    }));
});

server.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

### start.sh (Bash Script for Running)
```bash
#!/bin/bash

echo "Starting server..."

# Check if Node.js is installed
if ! command -v node &> /dev/null; then
    echo "Error: Node.js is not installed!"
    exit 1
fi

# Set environment variables
export PORT=3000
export NODE_ENV=development

# Run server
echo "Server will run on port $PORT"
node server.js
```
*- `#!/bin/bash` : This is called a `shebang`. It tells the system to run this script using the bash shell.
*- `command -v node`: checks if the command node exists in your system’s PATH.

*- If Node.js is installed, it returns success (exit code 0). If not, it fails.

*-` !` negates the result, so this condition means: “if Node.js is NOT found”.

*- `&> /dev/null`: hides any output (so you don’t see error messages from the check).
*- `exit 1`: stops the script immediately and returns an error code (1 means failure).

## Make file executable
```bash
chmod +x check-node.sh
```
## run the file
```bash
./check-node.sh
```

## permissions Type

r → read (you can open/read the file).

w → write (you can edit/modify/delete the file).

x → execute (you can run the file as a program/script).

### And these permissions apply to three categories of users:

1- User (u) → the owner of the file.

2- Group (g) → users in the same group as the file.

3- Others (o) → everyone else.

## check permissions
```bash
ls -l check-node.sh   //  -rwxr-xr--
```
- First character - → it’s a file (if it was d, it would be a directory).

- Next three rwx → permissions for the user (owner): read, write, execute.

- Next three r-x → permissions for the group: read, execute.

- Last three r-- → permissions for others: read only.

```bash
chmod u+x file   # add execute for user
chmod g-w file   # remove write for group
chmod o+r file   # add read for others
chmod ugo+r file # add read for everyone

```

Each permission is a number:

r = 4

w = 2

x = 1

You add them up:

7 = rwx (4+2+1)

6 = rw- (4+2)

5 = r-x (4+1)

4 = r--
```bash
chmod 755 file
```
Means:

7 → user = rwx

5 → group = r-x

5 → others = r-x


................................


## Advanced Challenges

### Challenge 1: Bash Script for Monitoring
Write a script that monitors whether the server is running or not

### Challenge 2: Log Management
Write a script that saves server logs in separate files

### Challenge 3: Auto Deployment
Write a script that pulls latest updates from Git and restarts the server

---

## Arabic Version

# الدرس الأول: أساسيات الإنترنت وكيف يعمل

## أهداف الدرس
- فهم كيف يعمل الإنترنت
- معرفة الـ HTTP Protocol
- فهم الـ DNS
- التعرف على TCP/IP
- أساسيات Command Line

## 1. ما هو الإنترنت؟

الإنترنت هو شبكة ضخمة من أجهزة الكمبيوتر المترابطة حول العالم. تخيل إنه زي شبكة الطرق، بس بدل العربيات، البيانات هي اللي بتتحرك.

### مكونات الإنترنت الأساسية:
- **الخوادم (Servers)**: أجهزة كمبيوتر قوية تحتوي على مواقع الويب
- **العملاء (Clients)**: أجهزة المستخدمين (الموبايل، اللاب توب)
- **البروتوكولات (Protocols)**: القواعد اللي بتنظم التواصل

## 2. كيف تعمل طلبات HTTP؟

لما تكتب www.google.com في المتصفح، بيحصل الآتي:

```
1. المتصفح يرسل HTTP Request
2. الخادم يستقبل الطلب
3. الخادم يرسل HTTP Response
4. المتصفح يعرض الصفحة
```

### مثال عملي - HTTP Request:
```http
GET /search?q=fullstack HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0
Accept: text/html
```

### HTTP Response:
```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<!DOCTYPE html>
<html>...</html>
```

## 3. نظام DNS - دليل الإنترنت

DNS زي دليل التليفون للإنترنت. بيحول الأسماء إلى عناوين IP.

### عملية DNS Lookup:
1. تكتب: `facebook.com`
2. DNS يترجمها إلى: `157.240.241.35`
3. المتصفح يتصل بالعنوان دا

```javascript
// مثال بسيط في JavaScript
console.log('نريد الوصول إلى: facebook.com');
console.log('DNS يترجم إلى: 157.240.241.35');
console.log('الاتصال تم!');
```

## 4. بروتوكول TCP/IP

TCP/IP هو اللغة اللي أجهزة الكمبيوتر بتكلم بيها بعض:

### TCP (Transmission Control Protocol):
- يضمن وصول البيانات كاملة
- يرتب البيانات في الترتيب الصحيح
- زي البريد المضمون

### IP (Internet Protocol):  
- يحدد عنوان كل جهاز
- زي عنوان البيت

## 5. مثال عملي - كيف يعمل موقع بسيط

```javascript
// مثال بسيط بـ Node.js
const http = require('http');

// إنشاء خادم
const server = http.createServer((req, res) => {
    // استقبال طلب من العميل
    console.log('طلب جديد:', req.url);
    
    // إرسال رد
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    res.end('<h1>مرحبا! أول خادم لي</h1>');
});

// تشغيل الخادم على البورت 3000
server.listen(3000, () => {
    console.log('الخادم يعمل على http://localhost:3000');
});
```

## 6. أنواع HTTP Methods المهمة

```javascript
// GET - لجلب البيانات
GET /users          // جلب كل المستخدمين

// POST - لإضافة بيانات جديدة  
POST /users         // إضافة مستخدم جديد

// PUT - لتحديث البيانات
PUT /users/123      // تحديث المستخدم رقم 123

// DELETE - لحذف البيانات
DELETE /users/123   // حذف المستخدم رقم 123
```

## 7. Status Codes المهمة

```
200 OK          - كل شيء تمام
201 Created     - تم إنشاء البيانات بنجاح  
400 Bad Request - طلب خاطئ
401 Unauthorized- غير مخول
404 Not Found   - الصفحة غير موجودة
500 Server Error- خطأ في الخادم
```

## 8. تمرين عملي

قم بإنشاء خادم بسيط يرد على طلبات مختلفة:

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
    const pathname = url.parse(req.url).pathname;
    
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    
    switch(pathname) {
        case '/':
            res.end('<h1>الصفحة الرئيسية</h1>');
            break;
        case '/about':
            res.end('<h1>من نحن</h1>');
            break;
        case '/contact':
            res.end('<h1>تواصل معنا</h1>');
            break;
        default:
            res.writeHead(404);
            res.end('<h1>الصفحة غير موجودة</h1>');
    }
});

server.listen(3000);
```

## الواجب المنزلي

1. جرب الكود اللي فوق واشغله
2. جرب تضيف صفحات جديدة
3. ادرس الـ Status Codes وحفظهم
4. اقرأ عن HTTP Headers وأنواعها

## اختبار سريع

### أسئلة:
1. ما وظيفة DNS؟
2. ما الفرق بين GET و POST؟
3. ما معنى Status Code 404؟
4. اكتب مثال على HTTP Request بسيط

### الإجابات:
1. DNS يحول أسماء المواقع إلى عناوين IP
2. GET لجلب البيانات، POST لإضافة بيانات جديدة
3. الصفحة أو المورد غير موجود
4. GET /users HTTP/1.1, Host: example.com

## 9. أساسيات Command Line و Bash

### أوامر أساسية مهمة:
```bash
# التنقل في المجلدات
pwd                 # أين أنا الآن؟
ls                  # ما الملفات الموجودة؟
cd folder_name      # الانتقال لمجلد
cd ..              # العودة للمجلد السابق

# إدارة الملفات
mkdir new_folder    # إنشاء مجلد جديد
touch file.txt      # إنشاء ملف فارغ
cp file1.txt file2.txt  # نسخ ملف
mv old.txt new.txt  # إعادة تسمية أو نقل
rm file.txt         # حذف ملف
rm -rf folder/      # حذف مجلد وكل محتوياته

# عرض محتوى الملفات
cat file.txt        # عرض محتوى ملف
head file.txt       # أول 10 أسطر
tail file.txt       # آخر 10 أسطر
grep "text" file.txt # البحث في الملف
```

### مثال على Bash Script بسيط:
```bash
#!/bin/bash

# script لإنشاء مشروع Node.js جديد
echo "إنشاء مشروع جديد..."

# إنشاء المجلد
mkdir my-project
cd my-project

# إنشاء الملفات الأساسية
npm init -y
touch index.js
touch .gitignore

# إضافة محتوى أساسي
echo "console.log('مرحبا بالمشروع الجديد!');" > index.js
echo "node_modules/" > .gitignore

echo "المشروع جاهز!"
```

## 10. متغيرات البيئة (Environment Variables)

```bash
# تعيين متغير
export DATABASE_URL="mongodb://localhost:27017/myapp"
export PORT=3000

# استخدام المتغيرات في Node.js
# process.env.DATABASE_URL
# process.env.PORT
```

## 11. مثال متكامل - خادم مع Bash Management

### server.js
```javascript
const http = require('http');
const port = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
    res.writeHead(200, {'Content-Type': 'application/json'});
    res.end(JSON.stringify({
        message: 'الخادم يعمل!',
        port: port,
        time: new Date().toISOString()
    }));
});

server.listen(port, () => {
    console.log(`الخادم يعمل على البورت ${port}`);
});
```

### start.sh (Bash Script للتشغيل)
```bash
#!/bin/bash

echo "بدء تشغيل الخادم..."

# فحص إذا كان Node.js مثبت
if ! command -v node &> /dev/null; then
    echo "خطأ: Node.js غير مثبت!"
    exit 1
fi

# تعيين متغيرات البيئة
export PORT=3000
export NODE_ENV=development

# تشغيل الخادم
echo "الخادم سيعمل على البورت $PORT"
node server.js
```

## التحديات المتقدمة

### تحدي 1: Bash Script للمراقبة
اكتب script يراقب إذا كان الخادم يعمل أم لا

### تحدي 2: Log Management
اكتب script يحفظ logs الخادم في ملفات منفصلة

### تحدي 3: Auto Deployment
اكتب script يسحب آخر تحديثات من Git ويعيد تشغيل الخادم
