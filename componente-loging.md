Un componente login es parte de un aplicativo web, el cual sirve como filtro de acceso a la plataforma, este componente puede incluir validaciones en el mismo front
la validaciones serán:
   - Ingresar un correo electrónico
   - Ingresar una contraseña de máximo 8 caracteres
   - Habilitar botón hasta que se ingresen formatos correctos

Se conectara a un endpoint back para esto se espera un respuesta correcta 

iniciamos creando el componente con el siguiente comando (para esto se tendrá que tener un proyecto Angular creado)

```
ng g c loginComponent

```

Seguido nos dirigimos al archivo **app-routing.module.ts** e ingresamos el siguiente código:

```ts
const routes: Routes = [
  {path:'login',component:LoginComponent}
];
```

El paso anterior es para acceder a la ruta **http://localhost:4200/login** desde el navegador 

Ya teniendo acceso a la vista iniciaremos con la parte lógica, en el archivo **loginComponente.ts**. En este archivo ingresamos variables, métodos y manejo de excepciones

Iniciamos con **FormGroup** Agrupa varios FormControl en una unidad coherente. Esto es útil para gestionar formularios complejos con múltiples campos

Dentro declaramos los **FormControl** dentro del FormControl.

Para la declaración de una variable

```ts
  /** Se crea el formulario login **/
  formulario Login !: FormGroup;
```

Para buenas prácticas se declara los FormControl dentro de la variable formularioLogin y todo esto en un método

```ts
  creaFormularioLogin(){
      this.formularioLogin = this.fb.group({
    correo:[null,[Validators.required,Validators.email]],
    contrasenia:[null,[Validators.required]]
  }
```

> Nota: Se ingresa dentro de un método para la flexibilidad del código

Para que al momento de cargar el componente en el navegador, se cree el formulario **En caso de no cargar el formulario dejará de funcionar el componente**

```ts
  /** Constructor para la instancia de objetos antes de ejecutar el componente **/
  constructor(private fb: FormBuilder,
              private loginMapper: LoginMapper) {
                this.creaFormularioLogin();
              }
```

Notemos como dentro del formularioLogin se declará 2 FormControl:
  - correo
  - contrasenia

> Serán los campos a manipular por parte del cliente 

Seguido un arreglo **[]** donde la declaración **null** es el valor predeterminado, Validators.required declaración de que es obligatorio y solo en correo Validators.email que debe ser de tipo correo electrónico

Nos dirigimos al html loginComponent.html donde se verá la introducción de valores para el formulario iniciamos con una etiqueta de tipo **form** ingresando qué será de tipo **formGroup** y será igual al nombre de la variable que se declaró en el ts **formularioLogin**

```html
<form [formGroup]="formularioLogin">
</>
```

Con esto el html interpretará que se manipulara este formulario, ahora los input donde se ingresarán los valores, iniciando con el correo, declaramos un input donde ingresará el FormControl de correo
junto con un etiqueta 
```html

            <div class="col-md-5">
                <label class="etiqueta">correo</label>
                
            </div>

            <div class="col-md-6">
                <input type="text" 
                       class="form-control" 
                       placeholder="Ingrese el correo"
                       formControlName="correo"
                       >
            </div>
```

Observamos como la parte formControlName="correo" debe ser el mismo nombre del valor dentro del **formularioLogin**

Ahora la validaciones **NO SON PENDEJAS** se utilizarán para que el usuario vea donde la esta **CAGANDO** 


```html

        <!-- Mensaje de error cuando se deja en blanco el campo de correo -->
        <div 
        style="display: flex; justify-content: center"
        *ngIf="formularioLogin.get('correo')?.
                hasError('required') && formularioLogin.get('correo')?.
                touched">
        El correo es obligatorio.
        </div>
```

Se declara un div donde se da un estilo (depende el cliente se muestra el estilo del error, se acostumbra rojo y letras pequeñas).
Se declara una condicional con él un *ngIf el cual entra en función el obtener el valor del campo **correo** del formulario **formularioLogin** con un get y el  **hasError** obtiene un tipo de error en este caso el **required** Que fue el **Validators.required** declarado en el ts
Dentro del div la etiqueta **El correo es obligatorio.**

Este ejemplo es para los campos requeridos, se aplicará lo mismo en el caso del campo **contrasenia**


Ahora otra validación es el tipo de email que se declaró como **Validators.email** 

```html
        <!-- Mensaje de error cuando se ingresa un formato no válido en el campo de correo -->
        <div 
        style="display: flex; justify-content: center"
        *ngIf="formularioLogin.get('correo')?.
                hasError('email') && formularioLogin.get('correo')?.
                touched">
        Deber ser de formato correo.
        </div>
```

>Nota: Se puede ver como cambia el obtener el **hasError** y ahora se coloca el **email**

Ahora para el envío del los datos será por medio de un modelo el json quedará:

```json
{
  "correo": "",
  "contrasenia": ""
}

```

Para esto creará una interface con el nombre **LoginRequest** y dentro los valores a utilizar:

```ts
export interface LoginRequest{

    correo:string;
    contrasenia:string;
}
```

Ahora para la asignación del formulario con la interface **Se hacen mapper** que son **buenas prácticas** y no parecer ***Pendejo** enfrente de los master como yo

Para esto se crea una clase con el nombre **LoginMapper** (Se utilizan lo mapper para todo tipo de formularios, así que no se hace solo uno sino uno por cada regla de negocio)

Dentro de la clase se declara métodos que reciban un **FormGroup** y devuelva un modelo request, ejemplo:

```ts
    /** 
     * @description Se reibe un formulario para devolver un modelo request de login
     * 
     * @returns {LoginRequest} - Modelo para una solicitud de inicio de sesión
     * 
     * **/
    mapperToLoginRequest(formLogin: FormGroup):LoginRequest{

        const loginRequest: LoginRequest ={
            correo: formLogin.get('correo')?.value || '',
            contrasenia: formLogin.get('contrasenia')?.value || ''
        };

        return loginRequest;
    }

```

Y si notamos en el constructor ya lo ingresamo, ya se... te dio error relajate todo tivio se lo que hago

Este mapper debe entrar en función cuando se de el clic de inicio de sesión donde el formulari ya sea valido con sus campos

Para esto el botón debe deshabilitarse si la cagan con las validaciones, el botón se declara en el html de la siguiente manera:

```html
        <div class="row mt-3" style="display: flex; justify-content: flex-end">

            <div class="col-md-5">
                <button class="btn btn-success"
                [disabled]="formularioLogin.invalid"
                (click)="iniciarSesion()"
                > Iniciar sesión</button>
            </div>

        </div>
```

Con el código **[disabled]="formularioLogin.invalid"** está interpretando que se habilite o no, siempre y cuando el formulario no tenga validadores en falso
y el **(click)** llama el método iniciarSesion() que lo dejaramos de la siguiente manera:

```ts
  /** Obtiene los valores del formulario para mappearlos a un modelo de salida
   * 
   * @param {FormGroup} formLogin formulario donde se recibe el usuario y contrasena
   *  **/
  iniciarSesion(){
    //this.loginMapper.mapperToLoginRequest(this.formLogin);
    console.log(this.loginMapper.mapperToLoginRequest(this.formularioLogin));
    
  }
```

Observa cómo entra en función el **Mapper** tomando los valores del formulario y regresando un modelo request listo para enviarlo al endpoint, por el momento lo imprimimos en consola.



TE DEJO LOS ARCHIVO COMPLETOS

```ts
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { LoginMapper } from 'src/app/utileria/mapper/LoginMapper';

/**
 * @class LoginComponent.ts
 * 
 * @description Componente que contiene la logica para el incio de sesión 
 * 
 * @author Irvin Enrique Camacho Gonzalez
 * 
 */
@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {

  /** Se crea el formulario login **/
  formularioLogin !: FormGroup;

  /** Constructor para la instacia de objetos antes de ejecutar el componente **/
  constructor(private fb: FormBuilder,
              private loginMapper: LoginMapper) {
                this.creaFormularioLogin();
              }




  creaFormularioLogin(){
      this.formularioLogin = this.fb.group({
    correo:[null,[Validators.required,Validators.email]],
    contrasenia:[null,[Validators.required]]
  }
  )
  }
  /** Obtiene los valores del formulario para mappearlos a un modelo de salida
   * 
   * @param {FormGroup} formLogin formulario donde se recibe el usuario y contrasena
   *  **/
  iniciarSesion(){
    //this.loginMapper.mapperToLoginRequest(this.formLogin);
    console.log(this.loginMapper.mapperToLoginRequest(this.formularioLogin));
    
  }
}


```

```html
<div class="container">
    <div>
        <label class="tema mb-4" style="display: flex; justify-content: center"
        >Inicio de sesión</label>
    </div>

    <!-- Se asigna el formulario creado en loginComponent.ts para validar campos-->
    <form [formGroup]="formularioLogin">
        <div class="row">
            <div class="col-md-5">
                <label class="etiqueta">correo</label>
                
            </div>
            <div class="col-md-6">
                <input type="text" 
                       class="form-control" 
                       placeholder="Ingrese el correo"
                       formControlName="correo"
                       >
            </div>
        </div>

        <!-- Mensaje de error cuando se deja en blanco el campo de correo -->
        <div 
        style="display: flex; justify-content: center"
        *ngIf="formularioLogin.get('correo')?.
                hasError('required') && formularioLogin.get('correo')?.
                touched">
        El correo es obligatorio.
        </div>

        <!-- Mensaje de error cuando se ingresa un formato no valido en el campo de correo -->
        <div 
        style="display: flex; justify-content: center"
        *ngIf="formularioLogin.get('correo')?.
                hasError('email') && formularioLogin.get('correo')?.
                touched">
        Deber ser de formato correo.
        </div>


        <div class="row mt-3">
            <div class="col-md-5">
                <label class="etiqueta">contraseña</label>
                
            </div>
            <div class="col-md-6">
                <input class="form-control" 
                        type="password" 
                        placeholder="Ingrese la contraseña"
                        formControlName="contrasenia"
                        >
            </div>
        </div>

        <!-- Mensaje de error cuando se deja en blanco el campo de contraseña -->
        <div 
        style="display: flex; justify-content: center"
        *ngIf="formularioLogin.get('contrasenia')?.
                hasError('required') && formularioLogin.get('contrasenia')?.
                touched">
        La contraseña es obligatoria.
        </div>


        <div class="row mt-3" style="display: flex; justify-content: flex-end">

            <div class="col-md-5">
                <button class="btn btn-success"
                [disabled]="formularioLogin.invalid"
                (click)="iniciarSesion()"
                > Iniciar sesión</button>
            </div>

        </div>
    </form>

</div>

```
