# FORMULARIOS

## Introducción

Los formularios son una parte fundamental de cualquier aplicación web moderna, permitiendo a los usuarios interactuar con la aplicación ingresando datos. En Angular, existen dos enfoques principales para trabajar con formularios: formularios basados en template y formularios reactivos. Cada uno tiene sus propias características y ventajas dependiendo de los requisitos y la complejidad del formulario.

## Formularios en Template (ngModel)

Los formularios en template, a menudo conocidos como formularios basados en ngModel, utilizan la directiva `ngModel` para vincular los campos de entrada de HTML con propiedades del componente de Angular. Este enfoque es ideal para formularios simples y rápidos de implementar.

**Ejemplo de un formulario en template:**

1. **Componente de Angular:**
   ```typescript
   import { Component } from '@angular/core';
   
   @Component({
     selector: 'app-template-form',
     templateUrl: './template-form.component.html'
   })
   export class TemplateFormComponent {
     public username: string = '';
     public password: string = '';
     
     onSubmit() {
       console.log('Formulario enviado');
       console.log('Usuario:', this.username);
       console.log('Contraseña:', this.password);
     }
   }
   ```

2. **Template HTML (`template-form.component.html`):**
   ```html
   <form (ngSubmit)="onSubmit()">
     <label for="username">Usuario:</label>
     <input type="text" id="username" name="username" [(ngModel)]="username">
     
     <label for="password">Contraseña:</label>
     <input type="password" id="password" name="password" [(ngModel)]="password">
     
     <button type="submit">Enviar</button>
   </form>
   ```

## Formularios Reactivos (ngForm)

Los formularios reactivos, utilizando `ngForm`, ofrecen una forma más programática y declarativa de manejar formularios en Angular. Este enfoque es particularmente útil cuando se necesita una lógica más compleja en el manejo de formularios, como validaciones dinámicas o formularios con campos dependientes.

**Ejemplo de un formulario reactivo:**

1. **Componente de Angular:**
   ```typescript
   import { Component } from '@angular/core';
   import { FormGroup, FormBuilder, Validators } from '@angular/forms';
   
   @Component({
     selector: 'app-reactive-form',
     templateUrl: './reactive-form.component.html'
   })
   export class ReactiveFormComponent {
     public myForm: FormGroup;
     
     constructor(private fb: FormBuilder) {
       this.myForm = this.fb.group({
         username: ['', Validators.required],
         password: ['', Validators.required]
       });
     }
     
     onSubmit() {
       if (this.myForm.valid) {
         console.log('Formulario enviado');
         console.log('Valor del formulario:', this.myForm.value);
       }
     }
   }
   ```

2. **Template HTML (`reactive-form.component.html`):**
   ```html
   <form [formGroup]="myForm" (ngSubmit)="onSubmit()">
     <label for="username">Usuario:</label>
     <input type="text" id="username" formControlName="username">
     <div *ngIf="myForm.controls['username'].invalid && myForm.controls['username'].touched">
       Usuario requerido
     </div>
     
     <label for="password">Contraseña:</label>
     <input type="password" id="password" formControlName="password">
     <div *ngIf="myForm.controls['password'].invalid && myForm.controls['password'].touched">
       Contraseña requerida
     </div>
     
     <button type="submit" [disabled]="!myForm.valid">Enviar</button>
   </form>
   ```

En este ejemplo de formulario reactivo, se utiliza `FormBuilder` para crear un `FormGroup` que define la estructura del formulario y las validaciones necesarias. El uso de `formControlName` vincula cada campo de entrada con su control correspondiente en el formulario reactivo.

## Conclusión

Tanto los formularios en template como los formularios reactivos son poderosas herramientas en Angular, cada una con sus propios casos de uso y beneficios. La elección entre ambos dependerá de la complejidad del formulario y de los requisitos específicos de validación y manejo de datos en la aplicación.