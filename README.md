# nodeJS-y-mariadb
Conexion de un servidor, con base de datos en Mysql, utilizando express y NodeJs

// Importro requerimientos
const DBConnector = require('./src/dbconnector5.js');
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');

// Inicializo las apps
const app = express();
const router = express.Router();

// Seteo Puerto
const port = process.env.PORT || 8484;

// Preparo el APP
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());
app.use(cors());

// Seteo Ruta principal
app.use('/', router);

// Configuro Rutas
router.route('/').get((req,res)=>{
  res.json("Nuesta API esta Funcionando")
});

router.route('/users').get(async(req,res)=>{
    result = await DBConnector.query("SELECT * FROM usuarios")
    res.json(result);
    
  });
  
  router.route('/user/:id').get(async(req,res)=>{
    result = await DBConnector.queryWithParams("SELECT * FROM usuarios WHERE id=?", [req.params.id])
    res.json(result);
  });

  router.route('/user/add').post(async(req, res) => {
      result = await DBConnector.queryWithParams(
          "INSERT INTO usuarios(dni, nombre, apellido, email) VALUES (?,?,?,?)",
          [req.body.dni, req.body.nombre, req.body.apellido, req.body.email]
      )
      res.json(result);
  });

  router.route('/user/delete/:id').get(async(req,res) => {
      result = await DBConnector.queryWithParams("DELETE FROM usuarios WHERE id=?", [req.params.id])
      res.json(result);
  });

  router.route('/user/update').post(async(req,res) =>
  {
      result = await DBConnector.queryWithParams(
          "UPDATE usuarios SET nombre=?,apellido=?,email=? WHERE ID=?",
          [req.body.nombre, req.body.apellido, req.body.email, req.body.id]
      )
      res.json(result);
  });



// Inicio la APP
app.listen(port);

// Muestro Puero en consola
console.log("Inicio en el puerto " + port);

//DBConnector.query("SELECT * FROM usuarios");
