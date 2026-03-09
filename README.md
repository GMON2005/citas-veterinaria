<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Sistema de Citas Veterinaria</title>

<style>

body{
    font-family: Arial;
    background:#0f172a;
    padding:20px;
    display:flex;
    justify-content:center;
}


/* MARCO VETERINARIA */

.container{
    background:white;
    padding:20px;
    border-radius:10px;
    width:400px;
    position:relative;
    z-index:1;
}

.container::before{
    content:"";
    position:absolute;
    top:-4px;
    left:-4px;
    right:-4px;
    bottom:-4px;
    border-radius:12px;
    z-index:-1;

    background:linear-gradient(
        45deg,
        #00b894,
        #55efc4,
        #74b9ff,
        #0984e3,
        #00cec9,
        #6c5ce7,
        #00b894
    );

    background-size:400%;
    animation:vete 6s linear infinite;
}

@keyframes vete{
0%{background-position:0%}
100%{background-position:400%}
}

input, textarea, button{
    width:100%;
    margin:5px 0;
    padding:8px;
}

button{
    font-size:16px;
    cursor:pointer;
}

.cita{
    background:#e9f7ef;
    padding:10px;
    margin-top:10px;
    border-radius:5px;
}

.oculto{
    display:none;
}

h2{
    text-align:center;
}

</style>
</head>

<body>

<div class="container">

<h2>🐾 Sistema Veterinaria 🐾</h2>

<div id="login">

<input type="text" id="user" placeholder="Usuario">
<input type="password" id="pass" placeholder="Contraseña">

<button onclick="login()">🐾 Entrar</button>
<button onclick="crearCuenta()">🐾 Crear Cuenta</button>

</div>


<div id="app" class="oculto">

<h2>🐾 Registro de Citas Veterinaria 🐾</h2>

<input type="text" id="nombre" placeholder="Nombre de la mascota">
<input type="date" id="fecha">
<input type="time" id="hora">
<textarea id="motivo" placeholder="Motivo"></textarea>

<button onclick="guardarCita()">🐾 Guardar Cita</button>

<h3>🐾 Citas</h3>

<div id="listaCitas"></div>

<button onclick="cerrarSesion()">🐾 Cerrar sesión</button>

</div>

</div>


<script>

let usuarioActual = null



function crearCuenta(){

let user = document.getElementById("user").value
let pass = document.getElementById("pass").value

let usuarios = JSON.parse(localStorage.getItem("usuarios")) || []

let existe = usuarios.find(u => u.user === user)

if(existe){
alert("Usuario ya existe")
return
}

usuarios.push({user, pass})

localStorage.setItem("usuarios", JSON.stringify(usuarios))

alert("Cuenta creada")

}



function login(){

let user = document.getElementById("user").value
let pass = document.getElementById("pass").value

let usuarios = JSON.parse(localStorage.getItem("usuarios")) || []

let encontrado = usuarios.find(u => u.user === user && u.pass === pass)

if(encontrado){

usuarioActual = user

document.getElementById("login").classList.add("oculto")
document.getElementById("app").classList.remove("oculto")

mostrarCitas()

}else{

alert("Datos incorrectos")

}

}



function cerrarSesion(){

usuarioActual = null

document.getElementById("login").classList.remove("oculto")
document.getElementById("app").classList.add("oculto")

}



function guardarCita(){

let nombre = document.getElementById("nombre").value
let fecha = document.getElementById("fecha").value
let hora = document.getElementById("hora").value
let motivo = document.getElementById("motivo").value

let citas = JSON.parse(localStorage.getItem("citas")) || []

let ahora = new Date()

let nueva = {

usuario: usuarioActual,
nombre,
fecha,
hora,
motivo,
registro: ahora.toLocaleString()

}

citas.push(nueva)

localStorage.setItem("citas", JSON.stringify(citas))

mostrarCitas()

}



function mostrarCitas(){

let citas = JSON.parse(localStorage.getItem("citas")) || []

let lista = document.getElementById("listaCitas")

lista.innerHTML = ""

citas
.filter(c => c.usuario === usuarioActual)
.forEach((cita, index)=>{

lista.innerHTML += `

<div class="cita">

<b>🐾 ${cita.nombre}</b><br>
Fecha: ${cita.fecha}<br>
Hora: ${cita.hora}<br>
Motivo: ${cita.motivo}<br>
Registrado: ${cita.registro}<br>

<button onclick="editarCita(${index})">🐾 Editar</button>
<button onclick="eliminarCita(${index})">🐾 Eliminar</button>

</div>

`

})

}



function eliminarCita(index){

let citas = JSON.parse(localStorage.getItem("citas"))

citas.splice(index,1)

localStorage.setItem("citas", JSON.stringify(citas))

mostrarCitas()

}



function editarCita(index){

let citas = JSON.parse(localStorage.getItem("citas"))

let nuevaFecha = prompt("Nueva fecha (YYYY-MM-DD)")
let nuevaHora = prompt("Nueva hora (HH:MM)")

if(nuevaFecha){
citas[index].fecha = nuevaFecha
}

if(nuevaHora){
citas[index].hora = nuevaHora
}

localStorage.setItem("citas", JSON.stringify(citas))

mostrarCitas()

}

</script>

</body>
</html>
