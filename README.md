## câu lệnh npm
    npm init 
        ----->(tạo file package.json)

## cài đặt express
    npm install --save-exact express@4.17.1
        --->(cài đặt express với bản 4.17.1)

        --cách dùng
        b1: import Express from "express";
        b2:const app = express();
        và app.method(path, handle)

## cài node mon
    -- npm i nodemon
    --vào file package.json thêm câu lệnh vào scripts
     "start": "nodemon --exec babel-node src/server.js"

## cài morgan 
    -- nó giúp ta biet phuong yhuc nao minh gui di va mot so thong tin khac
    --npm i morgan

    cách dùng 
    b1: var morgan = require('morgan')
    b2:app.use(morgan('combined'))
    
## file .gitignore dùng để ẩn nội dung khi đẩy lên github --> viết tên file là dc

## VIEW ENGINE (handlebar)
    -- dùng để hiển thị ra giao diện wed
    -- npm install express-handlebars
    -- npm i ejs

    -- import path from 'path'
    

    muốn sử dụng ta cần có đoạn code sau ở file server.js
    // setting ejs
        app.set("view engine", "ejs");
        app.set('views', path.join(__dirname, 'resources/views'))

    -- nó sẽ sử dụng view engine là ejs
    -- path _dirname là để build ra từ file nào

## config viewEjs
    import express from "express";

    const configViewEngine = (app) => {
        app.use(express.static('./src/public'))
        // cấu hình view engine là ejs
        app.set("view engine", "ejs");
        app.set("views", "./src/resources/views")
    }

    export default configViewEngine;

## sass - nodejs
***install**
--npm i node-sass

**cấu hình**
vào file package.json để config

thêm câu lệnh sau vào phần script như start

 "watch": "node-sass --watch src/resources/scss/app.scss src/public/css/app.css",

***chạy câu lệnh npm run watch ở terminal khác***



## cài đặt babel nó hỗ trợ các câu lệnh mới nhất ( import)
    npm install --save-exact body-parser@1.19.0 nodemon@2.0.12 @babel/core@7.15.5 @babel/node@7.15.4 @babel/preset-env@7.15.6

    * tạo file .babelrc  để config babel
            {
            "presets": [
                "@babel/preset-env"
            ]
        }

## STATIC FILES
    // static file
    app.use(express.static(path.join(__dirname, 'public')))
    --public là thư mục muốn static

## tạo cổng từ file khác
* tạo file .env để lưu cổng web  (ví dụ PORT = 8080)
    * và nodeJs muốn dùng phải cài đặt dotenv
    *npm install --save-exact dotenv@10.0.0
    
    ** muốn sử dụng cần
    -- thêm require('dotenv').config();
    vào file server.js và
    --const port = process.env.PORT
    

## mô hình mvc
action --> dispatcher --> function handler
app.get('/news', (req, res) => {
    res.render('news.ejs');
});

-- app.get('/news',... )  --> đây đc gọi là routes --> viết ở file khác


*** controller ***
-- app.get('/news',handle() )  --> function handle trong đây dc gọi là controller và nó dc viết ở file khác 

-- controller tương tác với view ở chỗ nó render ra file html-css trong thư mục view

-- cum này (req, res) => {
    res.render('news.ejs');
} được viết ở file controller sẽ đúng mô hình mvc


*** module ***
-- SỬ DỤNG DATABASE VỚI NODE.JS
-- npm i --save-exact mysql2@2.3
-- npm i mysql2

-- tạo file connectDB.js 
và thêm code này rồi export ra file homeController trong thư mục controller(homecontroller sẽ import)


***connectDB export file ra homeController***
const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    database: 'nodebasic'
});

-->export default connection;

***homeController import file vào và thêm câu lệnh trong mỗi layout muốn lấy database***
import connection from '../config/connectDB'
exports.home = function (req, res) {
    // lấy database
    let data = [];
    connection.query(
        'SELECT * FROM `users` ',
        function (err, results, fields) {
            results.map((row) => {
                data.push({
                    id: row.id,
                    email: row.email,
                    address: row.address,
                    name: row.name
                })
            })
            return res.render('index.ejs', { dataUser: JSON.stringify(data) });
        }
    );
}

***render database ra view ejs cho browser(web, chrome, firefox,...) nhìn thấy***
--- trong file index.ejs để có thể nhìn thấy ta thêm dataUser của file homecontroller vào
----bằng cách sau: <%= dataUser %>

## jsonWebToken - jwt
step1: npm i jsonwebtoken
step2: create file jwt and import
===> sign() --> tạo mã token
===> verify() --> check mã token nếu đúng thì cho qua sai thì dừng 
--> middleware jwt -->ứng dụng làm login,, phân quyền user 


-----> ví dụ:

            jwt.sign({data: 'foobar'}, 'secret', { expiresIn: '1h' });
            hoặc
            ----->user là data truyền vào 
            jwt.sign({data:user},process.env.ACCESS_TOKEN, {
                algorithm:"HS256",
                expiresIn:process.env.TOKEN_TIME_LIFE
            },

-----> ví dụ:
    jwt.verify(token, 'secret', function(err, decoded) {
  console.log(decoded.foo) // bar
});


## Cài đặt OpenSSL, sử dụng tạo private key và public key
===> mã hóa secret bthg là chuỗi kí tự thì ngta có thể dung tool tra ra rất nhanh 
nhưng với ssl thì nó là cả 1 đoạn văn bản rất khó để hack 
----> secret coi như là mật khẩu 

---> tải thư viện ssl để tạo mật khẩu bảo mật với ssl
===> https://slproweb.com/products/Win32OpenSSL.html
--> nếu cài các bản kia k dc thì cài bản 1.1 light

--> cấu hình set môi  trg
vào tìm kiếm của máy tìm: environment --> chọn edit -> environment variable --> chọn path của system variable -> chọn new -> dán câu này vào và ok --> C:\Program Files\OpenSSL-Win64\bin

---> check thử xem dc chưa --> cmd gõ openssl --> nếu ra openssl> là đã cài dc 

---> openssl generate private key and public key
hoặc
--->https://stackoverflow.com/questions/44474516/how-to-create-public-and-private-key-with-openssl
---> 2 link này có câu lệnh tạo private key

===> vào thư project tạo thư mục key -> vào thư mục key -> cmd --> dán câu lệnh này vào để tạo key

 --> tạo private key --> openssl genrsa -out private.pem 2048 --> không có thông tin gì
 --> tạo private key có thông tin --> openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr

 --> tạo public key từ private key --> openssl rsa -in keypair.pem -pubout -out publickey.crt
 --> tạo public key từ private key có thông tin --> openssl req -key domain.key -new -out publickey2.csr

-->https://www.npmjs.com/package/jsonwebtoken --> 
var privateKey = fs.readFileSync('private.key');
var token = jwt.sign({ foo: 'bar' }, privateKey, { algorithm: 'RS256' });
-->

==> ví dụ:
    var jwt = require('jsonwebtoken')
    var fs = require('fs')
    var privateKey = fs.readFileSync('./key/private.pem');
    // var token = jwt.sign({ foo: 'bar' }, privateKey, { algorithm: 'RS256' });
    // console.log(token);
    const token = "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE2ODM5OTE0NTB9.nIYW-YJ-YkqcvLTrcDCY6C37w6DTHVJJUxTot7U23MA8lgOyadsJgiqJKwwUdQ4od5IW550eskPQScg946nNyZxK_JjNvqmK0laomZPzW0ARb8gIEQr20R9u6u76N6683suPNM8QOcxHNjSE2rpX0SzzJQpUx9I22O1CDT66n-VNoETXXJtg2cBXjSomtgyax-_Nl1rLPwb1v2mIGCig5iShrlIDW5JMd6s8BzBBUQeiWYjX_7InBMg_1bA6w5IIksIdb0rTnzky8k7JHNk1hYshZlW4_ucG7LHBVySDZD06KbXKTVm6ynxlgjSsJ8jvKkyGPmkgKoAzoM8JU0iycQ"

    var cert = fs.readFileSync('./key/publickey.crt'); 
    const kq = jwt.verify(token, cert, { algorithms: ['RS256'] })
    console.log(kq);









