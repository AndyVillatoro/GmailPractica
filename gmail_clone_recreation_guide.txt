### **Guía de Recreación del Proyecto "Gmail Clone"**

**Objetivo:** Recrear la aplicación "Gmail Clone" basándonos en la estructura y el código de tu proyecto existente.

---

#### **Paso 1: Configuración Inicial del Proyecto**

1.  **Crear un Nuevo Proyecto Flutter:**
    Abre tu terminal o línea de comandos y ejecuta el siguiente comando para crear un nuevo proyecto Flutter. Asegúrate de estar en el directorio donde deseas que se cree el proyecto (por ejemplo, `C:\Users\andyf\lpEjercicios\IS513-II-PAC-2025\intro-flutter\`).

    ```bash
    flutter create gmail_clone_recreation
    ```

    Esto creará una nueva carpeta `gmail_clone_recreation` con la estructura básica de un proyecto Flutter.

2.  **Navegar al Directorio del Proyecto:**
    Ingresa al directorio del proyecto recién creado:

    ```bash
    cd gmail_clone_recreation
    ```

3.  **Actualizar `pubspec.yaml` (Dependencias):**
    Abre el archivo `pubspec.yaml` en la raíz de tu nuevo proyecto (`gmail_clone_recreation/pubspec.yaml`).

    Busca la sección `dependencies:` y añade las siguientes líneas, asegurándote de mantener la indentación correcta (dos espacios):

    ```yaml
    dependencies:
      flutter:
        sdk: flutter

      cupertino_icons: ^1.0.8
      go_router: ^16.0.0 # Añade esta línea
    ```

    Después de guardar el archivo, ejecuta en tu terminal para obtener las nuevas dependencias:

    ```bash
    flutter pub get
    ```

    **Explicación:**
    *   `cupertino_icons`: Proporciona un conjunto de iconos estilo iOS que a menudo se usan en aplicaciones Flutter para una apariencia consistente.
    *   `go_router`: Es un paquete popular para la gestión de rutas y navegación en aplicaciones Flutter, ofreciendo una API declarativa y fácil de usar.

---

#### **Paso 2: Estructura de Carpetas en `lib`**

Dentro de la carpeta `lib` de tu nuevo proyecto (`gmail_clone_recreation/lib`), crea la siguiente estructura de directorios:

```
lib/
├───main.dart
├───data/
│   └───emails_income.dart
└───src/
    ├───ejemplos/
    │   ├───botones.dart
    │   └───listas.dart
    ├───views/
    │   ├───home_page.dart
    │   ├───login_page.dart
    │   ├───perfil_page.dart
    │   └───fragments/ # Esta carpeta puede estar vacía por ahora, pero la creamos para mantener la estructura
    └───widgets/
        ├───header_menu.dart
        ├───item_email.dart
        ├───item_menu_bar.dart
        └───side_menu.dart
```

---

#### **Paso 3: Archivo `main.dart` (Punto de Entrada y Navegación)**

Abre el archivo `lib/main.dart` y reemplaza todo su contenido con el siguiente código. Este archivo configura la orientación de la pantalla y define las rutas de navegación usando `go_router`.

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:gmail_clone_recreation/src/views/home_page.dart';
import 'package:gmail_clone_recreation/src/views/login_page.dart';
import 'package:gmail_clone_recreation/src/views/perfil_page.dart';
import 'package:go_router/go_router.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();

  // Bloquear la orientación del dispositivo a vertical
  SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp,
    DeviceOrientation.portraitDown,
  ]);

  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: GoRouter(
        initialLocation: '/login', // La aplicación inicia en la pantalla de login
        routes: [
          GoRoute(
            name: 'home',
            path: '/home',
            builder: (context, state) => HomePage(),
            // Rutas hijas de "/home"
            routes: [
              GoRoute(
                name: 'perfil',
                path: 'perfil', // La ruta completa será /home/perfil
                builder: (context, state) {
                  return const PerfilPage();
                },
              ),
            ],
          ),
          GoRoute(
            name: 'login',
            path: '/login',
            builder: (context, state) => LoginPage(),
          ),
        ],
      ),
      title: 'Gmail Clone',
      debugShowCheckedModeBanner: false, // Opcional: para quitar el banner de "Debug"
    );
  }
}
```

**Explicación:**
*   `WidgetsFlutterBinding.ensureInitialized()`: Asegura que el framework Flutter esté inicializado antes de realizar cualquier operación.
*   `SystemChrome.setPreferredOrientations()`: Limita la orientación de la aplicación a vertical (tanto hacia arriba como hacia abajo), evitando que gire si el dispositivo se rota.
*   `MaterialApp.router`: Utiliza `go_router` para la gestión de rutas.
*   `initialLocation: '/login'`: Define la ruta inicial de la aplicación, en este caso, la pantalla de login.
*   `GoRoute`: Define cada ruta de la aplicación.
    *   `name`: Un nombre único para la ruta, útil para la navegación programática.
    *   `path`: La URL de la ruta.
    *   `builder`: La función que construye el widget de la pantalla para esa ruta.
*   Rutas anidadas: Observa cómo `perfil` es una ruta hija de `home`, lo que significa que su path completo será `/home/perfil`.

---

#### **Paso 4: Implementación de Vistas**

Ahora crearemos los archivos para las diferentes pantallas de la aplicación. Por ahora, serán versiones básicas para que el proyecto compile.

1.  **`lib/src/views/login_page.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';
    import 'package:go_router/go_router.dart';

    class LoginPage extends StatelessWidget {
      const LoginPage({super.key});

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Text('Login Page'),
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text('Bienvenido a la página de Login'),
                const SizedBox(height: 20),
                ElevatedButton(
                  onPressed: () {
                    // Navegar a la página de inicio
                    context.go('/home');
                  },
                  child: const Text('Ir a Home'),
                ),
              ],
            ),
          ),
        );
      }
    }
    ```
    **Explicación:** Esta es una página simple con un botón que te permite navegar a la ruta `/home` usando `context.go()`.

2.  **`lib/src/views/home_page.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';
    import 'package:go_router/go_router.dart';

    class HomePage extends StatelessWidget {
      const HomePage({super.key});

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Text('Home Page'),
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text('Bienvenido a la página de Inicio'),
                const SizedBox(height: 20),
                ElevatedButton(
                  onPressed: () {
                    // Navegar a la página de perfil (ruta anidada)
                    context.go('/home/perfil');
                  },
                  child: const Text('Ir a Perfil'),
                ),
                const SizedBox(height: 20),
                ElevatedButton(
                  onPressed: () {
                    // Volver a la página de login
                    context.go('/login');
                  },
                  child: const Text('Cerrar Sesión'),
                ),
              ],
            ),
          ),
        );
      }
    }
    ```
    **Explicación:** Una página de inicio con botones para navegar a la página de perfil (demostrando la ruta anidada) y para "cerrar sesión" (volviendo al login).

3.  **`lib/src/views/perfil_page.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';
    import 'package:go_router/go_router.dart';

    class PerfilPage extends StatelessWidget {
      const PerfilPage({super.key});

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Text('Perfil Page'),
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text('Esta es la página de Perfil'),
                const SizedBox(height: 20),
                ElevatedButton(
                  onPressed: () {
                    // Volver a la página anterior (Home)
                    context.pop();
                  },
                  child: const Text('Volver a Home'),
                ),
              ],
            ),
          ),
        );
      }
    }
    ```
    **Explicación:** Una página de perfil simple con un botón para volver a la página anterior en la pila de navegación usando `context.pop()`.

---

#### **Paso 5: Implementación de Datos**

1.  **`lib/data/emails_income.dart`**
    Crea este archivo y añade el siguiente contenido. Este archivo contendrá una lista de datos simulados para los correos electrónicos.

    ```dart
    List<Map<String, dynamic>> emails = [
      {
        'sender': 'Google',
        'subject': 'Security alert',
        'body': 'Your account was logged in from a new device.',
        'time': '10:30 AM',
        'read': false,
        'starred': false,
      },
      {
        'sender': 'John Doe',
        'subject': 'Meeting tomorrow',
        'body': 'Just a reminder about our meeting tomorrow at 9 AM.',
        'time': 'Yesterday',
        'read': true,
        'starred': true,
      },
      {
        'sender': 'Sarah Connor',
        'subject': 'Project Update',
        'body': 'The project is progressing well. I\'ll send a detailed report soon.',
        'time': '2 days ago',
        'read': false,
        'starred': false,
      },
      {
        'sender': 'LinkedIn',
        'subject': 'New job opportunities',
        'body': 'Based on your profile, we found some new job opportunities for you.',
        'time': '3 days ago',
        'read': true,
        'starred': false,
      },
      {
        'sender': 'Netflix',
        'subject': 'Your next binge-watch awaits!',
        'body': 'Check out the new series and movies added this week.',
        'time': '4 days ago',
        'read': false,
        'starred': true,
      },
      {
        'sender': 'Amazon',
        'subject': 'Your order has been shipped',
        'body': 'Your recent order #123-4567890-1234567 has been shipped and will arrive soon.',
        'time': '5 days ago',
        'read': true,
        'starred': false,
      },
      {
        'sender': 'Spotify',
        'subject': 'New music from your favorite artists',
        'body': 'Discover new releases and personalized playlists.',
        'time': '1 week ago',
        'read': false,
        'starred': false,
      },
      {
        'sender': 'GitHub',
        'subject': 'Pull request review requested',
        'body': 'John Doe requested your review on a pull request in repository "my-project".',
        'time': '1 week ago',
        'read': true,
        'starred': true,
      },
      {
        'sender': 'Duolingo',
        'subject': 'It\'s time to practice your Spanish!',
        'body': 'Keep up your streak and practice your language skills.',
        'time': '2 weeks ago',
        'read': false,
        'starred': false,
      },
      {
        'sender': 'Medium',
        'subject': 'Your weekly digest',
        'body': 'Here are some stories you might like based on your interests.',
        'time': '3 weeks ago',
        'read': true,
        'starred': false,
      },
    ];
    ```
    **Explicación:** Esta lista de mapas simula una base de datos de correos electrónicos, cada uno con propiedades como remitente, asunto, cuerpo, hora, estado de lectura y si está destacado.

---

#### **Paso 6: Implementación de Widgets**

Estos son los componentes reutilizables que formarán la interfaz de usuario.

1.  **`lib/src/widgets/header_menu.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';

    class HeaderMenu extends StatelessWidget {
      const HeaderMenu({super.key});

      @override
      Widget build(BuildContext context) {
        return Container(
          padding: const EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
          decoration: BoxDecoration(
            color: Colors.white,
            borderRadius: BorderRadius.circular(30.0),
            boxShadow: [
              BoxShadow(
                color: Colors.grey.withOpacity(0.2),
                spreadRadius: 1,
                blurRadius: 5,
                offset: const Offset(0, 3),
              ),
            ],
          ),
          child: Row(
            children: [
              IconButton(
                icon: const Icon(Icons.menu),
                onPressed: () {
                  Scaffold.of(context).openDrawer(); // Abre el Drawer
                },
              ),
              const Expanded(
                child: TextField(
                  decoration: InputDecoration(
                    hintText: 'Buscar en el correo',
                    border: InputBorder.none,
                    contentPadding: EdgeInsets.symmetric(horizontal: 10.0),
                  ),
                ),
              ),
              const CircleAvatar(
                backgroundImage: NetworkImage('https://via.placeholder.com/150'), // Placeholder de imagen de perfil
              ),
            ],
          ),
        );
      }
    }
    ```
    **Explicación:** Este widget representa la barra superior de búsqueda y el avatar del usuario, común en la interfaz de Gmail. Incluye un `IconButton` para abrir el menú lateral (Drawer).

2.  **`lib/src/widgets/item_email.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';

    class ItemEmail extends StatelessWidget {
      final Map<String, dynamic> email;

      const ItemEmail({super.key, required this.email});

      @override
      Widget build(BuildContext context) {
        return Container(
          padding: const EdgeInsets.symmetric(vertical: 8.0, horizontal: 16.0),
          color: email['read'] ? Colors.white : Colors.blue[50], // Fondo diferente si no está leído
          child: Row(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              CircleAvatar(
                backgroundColor: Colors.blueGrey,
                child: Text(
                  email['sender'][0].toUpperCase(),
                  style: const TextStyle(color: Colors.white),
                ),
              ),
              const SizedBox(width: 10),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      email['sender'],
                      style: TextStyle(
                        fontWeight: email['read'] ? FontWeight.normal : FontWeight.bold,
                      ),
                    ),
                    Text(
                      email['subject'],
                      style: TextStyle(
                        fontWeight: email['read'] ? FontWeight.normal : FontWeight.bold,
                      ),
                      maxLines: 1,
                      overflow: TextOverflow.ellipsis,
                    ),
                    Text(
                      email['body'],
                      style: const TextStyle(color: Colors.grey),
                      maxLines: 1,
                      overflow: TextOverflow.ellipsis,
                    ),
                  ],
                ),
              ),
              Column(
                crossAxisAlignment: CrossAxisAlignment.end,
                children: [
                  Text(
                    email['time'],
                    style: TextStyle(
                      fontSize: 12,
                      color: email['read'] ? Colors.grey : Colors.blue,
                    ),
                  ),
                  Icon(
                    email['starred'] ? Icons.star : Icons.star_border,
                    color: email['starred'] ? Colors.amber : Colors.grey,
                    size: 20,
                  ),
                ],
              ),
            ],
          ),
        );
      }
    }
    ```
    **Explicación:** Este widget representa un solo elemento de correo electrónico en la lista. Muestra el remitente, asunto, cuerpo, hora y un icono de estrella, cambiando el estilo si el correo ha sido leído o está destacado.

3.  **`lib/src/widgets/item_menu_bar.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';

    class ItemMenuBar extends StatelessWidget {
      final IconData icon;
      final String text;
      final bool isSelected;
      final VoidCallback onTap;

      const ItemMenuBar({
        super.key,
        required this.icon,
        required this.text,
        this.isSelected = false,
        required this.onTap,
      });

      @override
      Widget build(BuildContext context) {
        return Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: onTap,
            child: Container(
              padding: const EdgeInsets.symmetric(horizontal: 16.0, vertical: 12.0),
              decoration: BoxDecoration(
                color: isSelected ? Colors.blue.withOpacity(0.1) : Colors.transparent,
                borderRadius: const BorderRadius.horizontal(right: Radius.circular(30.0)),
              ),
              child: Row(
                children: [
                  Icon(icon, color: isSelected ? Colors.blue : Colors.grey[700]),
                  const SizedBox(width: 20),
                  Text(
                    text,
                    style: TextStyle(
                      color: isSelected ? Colors.blue : Colors.grey[700],
                      fontWeight: isSelected ? FontWeight.bold : FontWeight.normal,
                    ),
                  ),
                ],
              ),
            ),
          ),
        );
      }
    }
    ```
    **Explicación:** Un widget reutilizable para los elementos del menú lateral (Drawer), con un icono, texto y un estado `isSelected` para resaltar el elemento activo.

4.  **`lib/src/widgets/side_menu.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';
    import 'package:gmail_clone_recreation/src/widgets/item_menu_bar.dart';
    import 'package:go_router/go_router.dart';

    class SideMenu extends StatelessWidget {
      const SideMenu({super.key});

      @override
      Widget build(BuildContext context) {
        return Drawer(
          child: ListView(
            padding: EdgeInsets.zero,
            children: <Widget>[
              const DrawerHeader(
                decoration: BoxDecoration(
                  color: Colors.blue,
                ),
                child: Text(
                  'Gmail Clone',
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 24,
                  ),
                ),
              ),
              ItemMenuBar(
                icon: Icons.inbox,
                text: 'Recibidos',
                isSelected: true, // Por defecto, Recibidos está seleccionado
                onTap: () {
                  context.go('/home');
                  Navigator.pop(context); // Cierra el Drawer
                },
              ),
              ItemMenuBar(
                icon: Icons.star_border,
                text: 'Destacados',
                onTap: () {
                  // Lógica para navegar a Destacados
                  Navigator.pop(context);
                },
              ),
              ItemMenuBar(
                icon: Icons.send,
                text: 'Enviados',
                onTap: () {
                  // Lógica para navegar a Enviados
                  Navigator.pop(context);
                },
              ),
              ItemMenuBar(
                icon: Icons.drafts,
                text: 'Borradores',
                onTap: () {
                  // Lógica para navegar a Borradores
                  Navigator.pop(context);
                },
              ),
              const Divider(),
              ItemMenuBar(
                icon: Icons.settings,
                text: 'Configuración',
                onTap: () {
                  // Lógica para navegar a Configuración
                  Navigator.pop(context);
                },
              ),
              ItemMenuBar(
                icon: Icons.help_outline,
                text: 'Ayuda y comentarios',
                onTap: () {
                  // Lógica para Ayuda
                  Navigator.pop(context);
                },
              ),
              const Divider(),
              ItemMenuBar(
                icon: Icons.person,
                text: 'Perfil',
                onTap: () {
                  context.go('/home/perfil'); // Navega a la página de perfil
                  Navigator.pop(context); // Cierra el Drawer
                },
              ),
            ],
          ),
        );
      }
    }
    ```
    **Explicación:** Este widget es el menú lateral (Drawer) de la aplicación. Contiene una lista de `ItemMenuBar` para las diferentes secciones de Gmail, y un `DrawerHeader`. Observa cómo el botón "Perfil" navega a la ruta `/home/perfil`.

---

#### **Paso 7: Implementación de Ejemplos**

Estos archivos contienen ejemplos de widgets básicos de Flutter.

1.  **`lib/src/ejemplos/botones.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';

    class BotonesEjemplo extends StatelessWidget {
      const BotonesEjemplo({super.key});

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Text('Ejemplo de Botones'),
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                ElevatedButton(
                  onPressed: () {
                    // Acción al presionar el botón elevado
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('ElevatedButton presionado')),
                    );
                  },
                  child: const Text('Botón Elevado'),
                ),
                const SizedBox(height: 20),
                TextButton(
                  onPressed: () {
                    // Acción al presionar el botón de texto
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('TextButton presionado')),
                    );
                  },
                  child: const Text('Botón de Texto'),
                ),
                const SizedBox(height: 20),
                OutlinedButton(
                  onPressed: () {
                    // Acción al presionar el botón delineado
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('OutlinedButton presionado')),
                    );
                  },
                  child: const Text('Botón Delineado'),
                ),
                const SizedBox(height: 20),
                IconButton(
                  icon: const Icon(Icons.favorite),
                  onPressed: () {
                    // Acción al presionar el botón de icono
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('IconButton presionado')),
                    );
                  },
                  tooltip: 'Me gusta',
                ),
                const SizedBox(height: 20),
                FloatingActionButton(
                  onPressed: () {
                    // Acción al presionar el FAB
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('FloatingActionButton presionado')),
                    );
                  },
                  child: const Icon(Icons.add),
                ),
              ],
            ),
          ),
        );
      }
    }
    ```
    **Explicación:** Este archivo muestra diferentes tipos de botones en Flutter (`ElevatedButton`, `TextButton`, `OutlinedButton`, `IconButton`, `FloatingActionButton`) y cómo responder a sus eventos de presión.

2.  **`lib/src/ejemplos/listas.dart`**
    Crea este archivo y añade el siguiente contenido:

    ```dart
    import 'package:flutter/material.dart';

    class ListasEjemplo extends StatelessWidget {
      const ListasEjemplo({super.key});

      final List<String> items = const [
        'Elemento 1',
        'Elemento 2',
        'Elemento 3',
        'Elemento 4',
        'Elemento 5',
        'Elemento 6',
        'Elemento 7',
        'Elemento 8',
        'Elemento 9',
        'Elemento 10',
      ];

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Text('Ejemplo de Listas'),
          ),
          body: ListView.builder(
            itemCount: items.length,
            itemBuilder: (context, index) {
              return Card(
                margin: const EdgeInsets.all(8.0),
                child: ListTile(
                  leading: CircleAvatar(
                    child: Text('${index + 1}'),
                  ),
                  title: Text(items[index]),
                  subtitle: Text('Descripción del ${items[index]}'),
                  onTap: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Clic en ${items[index]}')),
                    );
                  },
                ),
              );
            },
          ),
        );
      }
    }
    ```
    **Explicación:** Este archivo demuestra cómo crear una lista desplazable en Flutter usando `ListView.builder`, que es eficiente para listas largas ya que solo construye los elementos visibles.

---

#### **Paso 8: Ejecutar y Verificar**

Una vez que hayas creado todos los archivos y pegado el contenido, puedes ejecutar tu aplicación.

1.  **Asegúrate de estar en el directorio raíz de tu nuevo proyecto (`gmail_clone_recreation`).**
2.  **Ejecuta la aplicación:**

    ```bash
    flutter run
    ```

    Esto iniciará la aplicación en un emulador o dispositivo conectado. Deberías ver la pantalla de login, y poder navegar a Home y Perfil.

---

**Próximos Pasos y Consideraciones:**

*   **Integración de Vistas y Widgets:** Actualmente, las vistas `home_page.dart`, `login_page.dart` y `perfil_page.dart` son muy básicas. El siguiente paso sería integrar los widgets `HeaderMenu`, `SideMenu` y `ItemEmail` en `home_page.dart` para construir la interfaz de usuario completa de Gmail.
*   **Gestión de Estado:** Para una aplicación más compleja como Gmail, necesitarías una gestión de estado más robusta (por ejemplo, `Provider`, `Riverpod`, `Bloc`) para manejar el estado de los correos (leído/no leído, destacado), la navegación y otras interacciones.
*   **Diseño y Estilo:** Puedes refinar el diseño y los estilos para que se parezcan más a la aplicación real de Gmail.
*   **Funcionalidad Adicional:** Implementar la funcionalidad de búsqueda, envío de correos, etc.
