# 🎯 Detours - Microsoft API Interception Library

## 📋 Descripción

**Detours** es una potente biblioteca de Microsoft diseñada para interceptar y redirigir funciones de API de Windows en tiempo de ejecución. Esta implementación incluye integración con **Zydis**, un desensamblador de alta velocidad y precisión para arquitecturas x86/x86-64, proporcionando capacidades avanzadas de análisis de código y manipulación de instrucciones.

La biblioteca permite a los desarrolladores crear hooks dinámicos, interceptar llamadas de sistema, implementar parches en tiempo de ejecución y realizar análisis de código sin modificar los binarios originales.

## ✨ Características principales

- 🔌 **Interceptación de APIs**: Hook dinámico de funciones de Windows API
- 🏗️ **Arquitectura Multiplataforma**: Soporte para x86 y x64
- 🎯 **Trampolines Inteligentes**: Sistema avanzado de trampolines para preservar la funcionalidad original
- 🔍 **Integración con Zydis**: Desensamblado preciso de instrucciones x86/x86-64
- ⚡ **Rendimiento Optimizado**: Mínima sobrecarga en tiempo de ejecución
- 🛡️ **Gestión de Memoria Segura**: Protección y restauración automática de permisos de memoria
- 📊 **Soporte IAT**: Manipulación de Import Address Table (IAT)
- 🎨 **Flexibilidad de Hooking**: Múltiples métodos de interceptación (Jump, Call, Push/Ret)

## 📸 Capturas de pantalla

```
🚀 Ejemplo de uso básico:

// Función original
typedef int (WINAPI *MessageBoxFunc)(HWND, LPCSTR, LPCSTR, UINT);
MessageBoxFunc OriginalMessageBox = nullptr;

// Hook function
int WINAPI HookedMessageBox(HWND hWnd, LPCSTR lpText, LPCSTR lpCaption, UINT uType) {
    // Tu código personalizado aquí
    return OriginalMessageBox(hWnd, "¡Interceptado!", lpCaption, uType);
}

// Aplicar el hook
OriginalMessageBox = (MessageBoxFunc)DetourFunction(
    (uintptr_t)MessageBoxA, 
    (uintptr_t)HookedMessageBox
);
```

## 🛠️ Tecnologías utilizadas

- ![C++](https://img.shields.io/badge/C++-00599C?style=flat&logo=c%2B%2B&logoColor=white) **C++17** - Lenguaje principal
- ![Visual Studio](https://img.shields.io/badge/Visual%20Studio-5C2D91?style=flat&logo=visual-studio&logoColor=white) **Visual Studio 2019+** - IDE de desarrollo
- ![Windows](https://img.shields.io/badge/Windows-0078D6?style=flat&logo=windows&logoColor=white) **Windows API** - APIs del sistema
- 🔧 **Zydis Library** - Desensamblador de instrucciones
- 📦 **CMake/MSBuild** - Sistema de construcción
- 🧪 **Static Library** - Distribución como biblioteca estática

## 📦 Instalación y uso

### Prerrequisitos
- Windows 10/11
- Visual Studio 2019 o superior
- Windows SDK
- CMake 3.15+ (opcional)

### 🚀 Instalación

1. **Clona el repositorio**
```bash
git clone https://github.com/tu-usuario/detours.git
cd detours
```

2. **Compila el proyecto**
```bash
# Usando Visual Studio
MSBuild detours.sln /p:Configuration=Release /p:Platform=x64

# O usando el IDE
# Abre detours.sln en Visual Studio y compila
```

3. **Integra en tu proyecto**
```cpp
#include "Detours.h"
#pragma comment(lib, "detours.lib")
```

### 💡 Ejemplo de uso

```cpp
#include "Detours.h"

// Declaración de la función original
typedef BOOL (WINAPI *CreateFileFunc)(LPCSTR, DWORD, DWORD, LPSECURITY_ATTRIBUTES, DWORD, DWORD, HANDLE);
CreateFileFunc OriginalCreateFile = nullptr;

// Función hook
BOOL WINAPI HookedCreateFile(LPCSTR lpFileName, DWORD dwDesiredAccess, /* ... otros parámetros */) {
    // Tu lógica personalizada
    printf("Interceptado: %s\n", lpFileName);
    return OriginalCreateFile(lpFileName, dwDesiredAccess, /* ... */);
}

int main() {
    // Aplicar hook
    OriginalCreateFile = (CreateFileFunc)Detours::X64::DetourFunction(
        (uintptr_t)CreateFileA,
        (uintptr_t)HookedCreateFile
    );
    
    // Tu aplicación continúa...
    return 0;
}
```

## 📁 Estructura del proyecto

```
detours/
├── 📄 detours.sln              # Solución principal
├── 📂 detours/                 # Código fuente principal
│   ├── 🔧 Detours.h           # Archivo de cabecera principal
│   ├── ⚙️ Detours.cpp         # Implementación base
│   ├── 🏗️ Detours32.cpp       # Implementación x86
│   ├── 🏗️ Detours64.cpp       # Implementación x64
│   ├── 🛠️ stdafx.h            # Cabeceras precompiladas
│   ├── 🔗 HideStaticLibSymbols.c # Ocultación de símbolos
│   └── 📂 zydis/              # Biblioteca Zydis integrada
│       ├── 📂 src/            # Código fuente de Zydis
│       ├── 📂 tools/          # Herramientas de Zydis
│       └── 📂 assets/         # Recursos y configuración
└── 📋 detours.vcxproj         # Archivo de proyecto Visual Studio
```

## 👥 Autores / Colaboradores

- 🏢 **Microsoft Corporation** - Autores originales de Detours
- 🔍 **Florian Bernd** - Desarrollo de Zydis Disassembler Library
- 🛠️ **Comunidad Open Source** - Contribuciones y mejoras

### 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

```
MIT License

Copyright (c) Microsoft Corporation

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

## 📞 Contacto para empresas / Colaboraciones

### 🏢 Para consultas empresariales:

- 📧 **Email**: enterprise@detours-project.com
- 💼 **LinkedIn**: [Detours Project](https://linkedin.com/company/detours-project)
- 🌐 **Website**: [www.detours-project.com](https://www.detours-project.com)

### 🤝 Para colaboraciones técnicas:

- 🐛 **Issues**: [GitHub Issues](https://github.com/tu-usuario/detours/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/tu-usuario/detours/discussions)
- 📱 **Discord**: [Detours Community Server](https://discord.gg/detours-dev)

### 🎯 Servicios disponibles:

- 🔧 **Consultoría técnica** especializada en hooking y análisis de malware
- 🛡️ **Desarrollo de soluciones** de seguridad personalizadas
- 📚 **Training empresarial** en reverse engineering y API hooking
- 🚀 **Integración personalizada** para soluciones corporativas

---

<div align="center">

**⭐ Si este proyecto te ha sido útil, considera darle una estrella ⭐**

[![Stars](https://img.shields.io/github/stars/tu-usuario/detours?style=social)](https://github.com/tu-usuario/detours/stargazers)
[![Forks](https://img.shields.io/github/forks/tu-usuario/detours?style=social)](https://github.com/tu-usuario/detours/network/members)
[![Issues](https://img.shields.io/github/issues/tu-usuario/detours)](https://github.com/tu-usuario/detours/issues)

</div>
