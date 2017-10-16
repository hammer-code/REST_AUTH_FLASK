REST-auth
=========

Installation
------------
  setelah di cloning selanjutnya buat environment dengan virtualenv:

    $ virtualenv venv
    $ source venv/bin/activate
    (venv) $ pip install -r requirements.txt

  Khusus Pengguna Windows Ikuti Perintah Berikut Ini:

    $ virtualenv venv
    $ venv\Scripts\activate
    (venv) $ pip install -r requirements.txt

Running
-------

Untuk Menjalankan Aplikasi ketik perintah berikut ini:

    (venv) $ python api.py
     * Running on http://127.0.0.1:5000/
     * Restarting with reloader



API Documentation
-----------------

- POST **/api/users**

    Membuat User Baru.<br>


- GET **/api/users/&lt;int:id&gt;**

    Data user.<br>

- GET **/api/token**

    Mendapatkan Token. <br>

- GET **/api/resource**

    Data Yang Terlindungi. <br>

Example
-------
Menambahkan user Baru :
    $ curl -i -X POST -H "Content-Type: application/json" -d '{"username":"iank","password":"12345"}' http://127.0.0.1:5000/api/users
    HTTP/1.0 201 CREATED
    Content-Type: application/json
    Content-Length: 27
    Location: http://127.0.0.1:5000/api/users/1
    Server: Werkzeug/0.9.4 Python/2.7.3
    Date: Thu, 28 Nov 2013 19:56:39 GMT

    {
      "username": "iank"
    }

Melihat Kredensial Token:

    $ curl -u iank:12345 -i -X GET http://127.0.0.1:5000/api/resource
    HTTP/1.0 200 OK
    Content-Type: application/json
    Content-Length: 30
    Server: Werkzeug/0.9.4 Python/2.7.3
    Date: Thu, 28 Nov 2013 20:02:25 GMT

    {
      "data": "Hello, iank!"
    }

Contoh Kredensial Ketika password atau username salah:

    $ curl -u iank:54321 -i -X GET http://127.0.0.1:5000/api/resource
    HTTP/1.0 401 UNAUTHORIZED
    Content-Type: text/html; charset=utf-8
    Content-Length: 19
    WWW-Authenticate: Basic realm="Authentication Required"
    Server: Werkzeug/0.9.4 Python/2.7.3
    Date: Thu, 28 Nov 2013 20:03:18 GMT

    Unauthorized Access

Melihat Token:

    $ curl -u iank:12345 -i -X GET http://127.0.0.1:5000/api/token
    HTTP/1.0 200 OK
    Content-Type: application/json
    Content-Length: 139
    Server: Werkzeug/0.9.4 Python/2.7.3
    Date: Thu, 28 Nov 2013 20:04:15 GMT

    {
      "duration": 600,
      "token": "eyJhbGciOiJIUzI1NiIsImV4cCI6MTUwODEzODE1NiwiaWF0IjoxNTA4MTM3NTU2fQ.eyJpZCI6MX0.xJXiXRAn8V7RgrTSO2dj6Krj66PtJ2owQTjIzlir0NY"
    }

Melihat Data Yang Diproteksi:

    $ curl -u eyJhbGciOiJIUzI1NiIsImV4cCI6MTUwODEzODE1NiwiaWF0IjoxNTA4MTM3NTU2fQ.eyJpZCI6MX0.xJXiXRAn8V7RgrTSO2dj6Krj66PtJ2owQTjIzlir0NY:x -i -X GET http://127.0.0.1:5000/api/resource
    HTTP/1.0 200 OK
    Content-Type: application/json
    Content-Length: 30
    Server: Werkzeug/0.9.4 Python/2.7.3
    Date: Thu, 28 Nov 2013 20:05:08 GMT

    {
      "data": "Hello, iank!"
    }
