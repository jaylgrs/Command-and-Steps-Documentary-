# Java JDK 25 Setup Guide for Windows

A practical reference for installing JDK 25, switching Java versions, configuring Visual Studio Code, and verifying Java environments.

This guide is designed for developers who work on shared Windows computers where installed applications and environment settings may be reset after restarting.

## Table of Contents

1. [Install JDK 25](#1-install-jdk-25)
2. [Portable JDK Setup](#2-portable-jdk-setup)
3. [Verify the Java Installation](#3-verify-the-java-installation)
4. [Switch Java Versions](#4-switch-java-versions)
5. [Configure Visual Studio Code](#5-configure-visual-studio-code)
6. [Configure Gradle Projects](#6-configure-gradle-projects)
7. [Troubleshooting](#7-troubleshooting)
8. [Quick Command Reference](#8-quick-command-reference)
9. [Official Documentation](#9-official-documentation)

---

## 1. Install JDK 25

JDK 25 provides the compiler and runtime tools required to develop Java applications.

### Step 1: Download JDK 25

Download Eclipse Temurin JDK 25 from the official website.

**Download:** [Eclipse Temurin JDK 25](https://adoptium.net/temurin/releases/?version=25)

Select the following options:

- Operating System: Windows
- Architecture: x64 (for most standard Intel and AMD PCs)
- Package Type: JDK
- Version: 25
- Installer: MSI

Choose the appropriate architecture for your computer.

### Step 2: Run the installer

1. Open the downloaded MSI installer.
2. Follow the installation instructions.
3. If available, enable the options to add Java to `PATH` and configure `JAVA_HOME`.
4. Complete the installation.

Administrator permission may be required.

### Alternative: Install using WinGet

If WinGet is available, open PowerShell and run:

```powershell
winget install EclipseAdoptium.Temurin.25.JDK
```

**Note:** Installing JDK 25 on a shared PC does not guarantee that it will remain installed after a Deep Freeze reset.

---

## 2. Portable JDK Setup

A portable JDK is useful when working on shared computers that reset installed applications after restarting.

Instead of relying on a system-wide installation, keep an extracted JDK on storage that persists between sessions, such as a USB drive.

### Step 1: Download the ZIP package

Open the official JDK 25 download page:

[Download Eclipse Temurin JDK 25](https://adoptium.net/temurin/releases/?version=25)

Select Windows, x64, JDK, and the ZIP package.

### Step 2: Extract the archive

Extract the downloaded ZIP file to a location that will remain available.

For example:

```text
E:\Java\jdk-25
```

Replace `E:` with your actual USB drive letter.

The extracted folder should contain directories such as `bin`, `conf`, `include`, and `lib`.

The Java executables should be located at:

```text
E:\Java\jdk-25\bin\java.exe
```

```text
E:\Java\jdk-25\bin\javac.exe
```

If the archive extracts into a nested folder, use the actual directory containing `bin`.

### Important

A portable JDK is not automatically persistent just because it is extracted from a ZIP file.

Keep it on storage that survives the PC reset. If your USB drive letter changes, update the path in the commands below.

---

## 3. Verify the Java Installation

Open a new PowerShell terminal.

### Check the Java runtime

```powershell
java -version
```

### Check the Java compiler

```powershell
javac -version
```

### Check the current JAVA_HOME

```powershell
$env:JAVA_HOME
```

### Find the Java executable being used

```powershell
where.exe java
```

### Find the Java compiler being used

```powershell
where.exe javac
```

For JDK 25, the Java version and compiler version should both report version 25.

If the commands report another version, your terminal may be using a different Java installation.

---

## 4. Switch Java Versions

You can switch Java versions in PowerShell without changing the permanent system environment variables.

This is especially useful on shared Windows computers.

### Switch to JDK 25

The following example assumes that JDK 25 is located on drive `E:`.

#### Step 1: Set JAVA_HOME

```powershell
$env:JAVA_HOME = "E:\Java\jdk-25"
```

#### Step 2: Update PATH for the current terminal

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

#### Step 3: Verify the active version

```powershell
java -version
```

```powershell
javac -version
```

The current PowerShell session should now use JDK 25, provided the specified path is correct.

These changes apply only to the current PowerShell process and its child processes. They do not permanently change the Windows system environment.

### Switch back to JDK 21

If JDK 21 is available at the example location below, use:

```powershell
$env:JAVA_HOME = "E:\Java\jdk-21"
```

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify the version:

```powershell
java -version
```

Replace the example paths with your actual JDK locations.

**Tip:** Open a new PowerShell terminal after changing JDK versions if you want a clean environment.

---

## 5. Configure Visual Studio Code

VS Code can use different JDK installations for Java projects.

### Step 1: Install the Java extensions

Open VS Code and press:

`Ctrl + Shift + X`

Search for and install the **Extension Pack for Java**.

Official extension:

[Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

### Step 2: Configure the Java runtime

Press:

`Ctrl + Shift + P`

Search for:

`Java: Configure Java Runtime`

Inspect the JDK versions detected by VS Code.

### Step 3: Configure JDK 25 in settings.json

Open the Command Palette and search for:

`Preferences: Open User Settings (JSON)`

Add or merge the following setting into your existing JSON configuration:

```json
{
    "java.configuration.runtimes": [
        {
            "name": "JavaSE-25",
            "path": "E:\\Java\\jdk-25",
            "default": true
        }
    ]
}
```

Replace the example path with the actual JDK location.

If your settings file already contains other settings, do not replace the entire file. Add the `java.configuration.runtimes` property alongside the existing properties.

**Important:** VS Code's Java runtime configuration and the terminal's `JAVA_HOME` are separate settings. Changing one does not necessarily change the other.

If you use a USB-based JDK, make sure the drive is connected before opening VS Code.

---

## 6. Configure Gradle Projects

Minecraft mods and plugins commonly use Gradle. The JDK used by the terminal, the Gradle build process, and the Minecraft test environment may not always be the same.

### Step 1: Switch the terminal to JDK 25

```powershell
$env:JAVA_HOME = "E:\Java\jdk-25"
```

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

### Step 2: Verify Java

```powershell
java -version
```

### Step 3: Build the project

From the project directory, run:

```powershell
.\gradlew.bat build
```

This uses the project's Gradle Wrapper when it is available.

### Step 4: Check the build result

For most Gradle projects, generated JAR files are located in:

```text
build\libs\
```

The exact output depends on the project configuration.

### Important compatibility note

Not every Minecraft project requires JDK 25.

- Use the JDK version required by the project's Minecraft version, mod loader, and build configuration.
- A project may compile for an older Java target while using a newer JDK to run Gradle.
- Some projects require a specific Gradle version or a particular Java runtime.

Do not change the project's Java toolchain or target version just because JDK 25 is installed.

---

## 7. Troubleshooting

### Problem: `java` is not recognized

**Possible cause:** Java is not on the current terminal's PATH.

Set the JDK path again:

```powershell
$env:JAVA_HOME = "E:\Java\jdk-25"
```

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Then verify:

```powershell
java -version
```

### Problem: `javac` is not recognized

**Possible cause:** The terminal is not using a JDK or the JDK `bin` directory is missing from PATH.

Check that this file exists:

```text
E:\Java\jdk-25\bin\javac.exe
```

If the file exists, repeat the temporary environment setup above.

### Problem: Java still reports the wrong version

Check the executable locations:

```powershell
where.exe java
```

```powershell
where.exe javac
```

The first matching executable in PATH may be from another Java installation.

Set `JAVA_HOME` to the correct JDK and put its `bin` directory first in the current session's PATH.

### Problem: VS Code uses a different JDK

1. Open the Command Palette.
2. Run `Java: Configure Java Runtime`.
3. Check the configured JDK path.
4. Verify that the USB or installation folder still exists.
5. Restart the Java language server or reload VS Code if necessary.

### Problem: Gradle reports an incompatible Java version

Check the Gradle Wrapper version:

```powershell
.\gradlew.bat --version
```

Review the project's `gradle-wrapper.properties`, build scripts, and Java requirements.

A newer JDK is not always compatible with an older Gradle release.

---

## 8. Quick Command Reference

### Check Java

```powershell
java -version
```

### Check the compiler

```powershell
javac -version
```

### Find Java installations in PATH

```powershell
where.exe java
```

### Set JDK 25 for the current PowerShell session

```powershell
$env:JAVA_HOME = "E:\Java\jdk-25"
```

### Update the current session's PATH

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

### Check JAVA_HOME

```powershell
$env:JAVA_HOME
```

### Check the Gradle runtime

```powershell
.\gradlew.bat --version
```

### Build a Gradle project

```powershell
.\gradlew.bat build
```

---

## 9. Official Documentation

- [Eclipse Temurin JDK 25 Downloads](https://adoptium.net/temurin/releases/?version=25)
- [Eclipse Temurin Windows Installation](https://adoptium.net/installation/windows)
- [Java in Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Managing Java Projects in VS Code](https://code.visualstudio.com/docs/java/java-project)
- [Gradle Documentation](https://docs.gradle.org/current/userguide/userguide.html)

---

*Created as a practical reference for Java development on Windows, especially when working on shared computers with temporary environments.*

**End of Guide**
