# CARGA DE COMPONENTES EN DIFERIDO (LAZYLOAD)
## Cargas en diferido y no diferido (ChatGPT):
En Angular, la carga diferida (lazy loading) y la no diferida (eager loading) son estrategias utilizadas para gestionar cómo y cuándo se cargan los módulos de la aplicación. Estas estrategias son fundamentales para optimizar el rendimiento y la velocidad de carga de la aplicación, especialmente en aplicaciones grandes y complejas.

### Carga Diferida (Lazy Loading)

La carga diferida implica cargar módulos únicamente cuando son necesarios. Esto se hace generalmente en combinación con el enrutador de Angular, de manera que los módulos se cargan cuando el usuario navega a una ruta específica que requiere ese módulo. Esta estrategia mejora el rendimiento inicial de la aplicación al reducir el tamaño del paquete que se carga inicialmente.

**Ventajas:**
- Reduce el tiempo de carga inicial de la aplicación.
- Mejora la experiencia del usuario, ya que solo se cargan los módulos necesarios cuando se necesitan.
- Optimiza el uso de los recursos del navegador.

**Cómo Implementar:**
1. Crear módulos separados para diferentes partes de la aplicación.
2. Configurar las rutas para estos módulos utilizando la propiedad `loadChildren` en el enrutador de Angular.

Ejemplo:
```typescript
const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule)
  }
];
```

### Carga No Diferida (Eager Loading)

La carga no diferida, por otro lado, implica cargar todos los módulos de la aplicación de inmediato cuando se carga la aplicación. Todos los módulos especificados se importan y se inicializan al comienzo, lo cual puede aumentar el tiempo de carga inicial, pero garantiza que todos los recursos estén disponibles de inmediato.

**Ventajas:**
- Todos los módulos están disponibles inmediatamente después de cargar la aplicación.
- Simplifica la configuración del enrutador, ya que no es necesario gestionar la carga diferida.

**Cómo Implementar:**
Simplemente importa los módulos en el módulo raíz de la aplicación (usualmente `AppModule`).

Ejemplo:
```typescript
import { FeatureModule } from './feature/feature.module';

@NgModule({
  declarations: [
    // componentes
  ],
  imports: [
    BrowserModule,
    FeatureModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Comparación y Elección de Estrategia

La elección entre carga diferida y no diferida depende del tamaño y la complejidad de la aplicación, así como de las necesidades específicas de rendimiento.

- **Carga Diferida:** Ideal para aplicaciones grandes con muchas funcionalidades que no siempre son necesarias de inmediato. Mejora el rendimiento inicial y la experiencia del usuario.
- **Carga No Diferida:** Adecuada para aplicaciones más pequeñas o cuando la velocidad de respuesta inmediata de todos los módulos es más importante que la velocidad de carga inicial.

Implementar la estrategia adecuada puede hacer una gran diferencia en la usabilidad y eficiencia de tu aplicación Angular.



```ts
 { path: 'paises', loadChildren: () => import('./paises/paises.module').then((m) => m.PaisesModule) },
```
Claro, esta línea es una configuración de una ruta en Angular que utiliza la carga diferida para el módulo `PaisesModule`.

**Desglose:**

- `{ path: 'paises',`: Define una ruta con el camino `'paises'`. Esto significa que cuando el usuario navega a `tudominio.com/paises`, esta ruta será activada.
- `loadChildren: () => import('./paises/paises.module').then((m) => m.PaisesModule)`: Especifica que el módulo `PaisesModule` debe ser cargado de manera diferida cuando esta ruta es visitada.
  - `import('./paises/paises.module')`: Usa la función de importación dinámica de ES6 para cargar el archivo del módulo `paises.module`.
  - `.then((m) => m.PaisesModule)`: Una vez que el módulo es cargado, accede al módulo específico `PaisesModule` desde el objeto del módulo cargado (representado por `m`).

**En resumen:** Cuando un usuario navega a la ruta `'paises'`, Angular carga el módulo `PaisesModule` de manera diferida, mejorando el rendimiento inicial de la aplicación al no cargar este módulo hasta que realmente sea necesario.

Importante no importar ese módulo en `app.module.ts`.

Cada módulo en diferido tiene que tener su propio sistema de enrutamiento.
Por cada módulo lazy, un router.
