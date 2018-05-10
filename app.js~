var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');
var favicon = require('serve-favicon');
var session = require('express-session');

var messages = require('./middleware/messages');

var routes = require('./routes/index');
var users = require('./routes/users');
var entries = require('./routes/entries');
var validate = require('./middleware/validate');
var register = require('./routes/register');
var login = require('./routes/login');
var api = require('./routes/api');

//
var user = require('./middleware/user');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
app.set('json spaces', 2);

app.locals.user = {};

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cookieParser());
app.use(messages);
app.use(express.static(path.join(__dirname, 'public')));

app.use(session({
        secret: 'secret',
        resave: false, saveUninitialized: true
}));

app.use('/api', api.auth);
app.use(user);
app.get('/api/user/:id', api.user);

// app.use(express.session());
// app.use(express.static(__dirname + '/public'));
// app.use(user);
// app.use(messages);
// app.use(app.router);

app.get('/register', register.form);
app.post('/register', register.submit);

app.get('/login', login.form);
app.post('/login', login.submit);
app.get('/logout', login.logout);

app.use('/users', users);

app.get('/post', entries.form);
app.post('/post', 
        validate.required('entry[title]'),
        validate.lengthAbove('entry[title]', 4),
        entries.submit);

app.use('/', entries.list);

// catch 404 and forward to error handler

app.use(function(req, res, next) {
  next(createError(404));
});

// error handler

app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
