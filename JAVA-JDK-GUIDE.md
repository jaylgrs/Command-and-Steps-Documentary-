# Java JDK 25 Setup Guide for Windows

A practical reference for installing JDK 25, switching Java versions, configuring Visual Studio Code, and troubleshooting Java environments.

This guide is designed for developers working on shared Windows computers where installed applications, environment variables, and files may be reset after restarting.

It includes a practical setup for Windows computers where Oracle Java 8 takes priority over a newer JDK.

## Table of Contents

1. [Install JDK 25](#1-install-jdk-25)
2. [Verify the Java Installation](#2-verify-the-java-installation)
3. [Fix Java 8 Taking Priority](#3-fix-java-8-taking-priority)
4. [Switch Java Versions](#4-switch-java-versions)
5. [Configure Visual Studio Code](#5-configure-visual-studio-code)
6. [Configure Gradle Projects](#6-configure-gradle-projects)
7. [Portable JDK Setup](#7-portable-jdk-setup)
8. [Troubleshooting](#8-troubleshooting)
9. [Quick Command Reference](#9-quick-command-reference)
10. [Official Documentation](#10-official-documentation)

---

## 1. Install JDK 25

The Java Development Kit (JDK) provides the Java compiler, runtime, and development tools required to build Java applications.

### Step 1: Download JDK 25

Download Eclipse Temurin JDK 25 from the official website.

**Download:** [Eclipse Temurin JDK 25](https://adoptium.net/temurin/releases/?version=25)

Select the following options:

- Operating System: Windows
- Architecture: x64 for most standard Intel and AMD PCs
- Package Type: JDK
- Version: 25
- Package: MSI installer

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

### Step 3: Find the installed JDK

The installation directory depends on the installer and configuration.

For Eclipse Temurin, check:

```text
C:\Program Files\Eclipse Adoptium\
```

List the installed directories:

```powershell
Get-ChildItem "C:\Program Files\Eclipse Adoptium" -Directory
```

Find the Java compiler:

```powershell
Get-ChildItem "C:\Program Files\Eclipse Adoptium" -Recurse -Filter javac.exe -ErrorAction SilentlyContinue |
    Select-Object -ExpandProperty FullName
```

The output should identify a `javac.exe` inside a JDK's `bin` directory.

For example:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\javac.exe
```

The JDK root directory is the parent directory of `bin`.

For the example above:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot
```

Use the actual path reported by your computer.

**Important:** Installing a newer JDK does not guarantee that Windows will use it. An older Java installation may still take priority in `PATH`.

---

## 2. Verify the Java Installation

Open PowerShell.

### Check the Java runtime

```powershell
java -version
```

### Check the Java compiler

```powershell
javac -version
```

### Check JAVA_HOME

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

For JDK 25, the runtime and compiler should report version 25.

Example:

```text
openjdk version "25.0.4"
```

```text
javac 25.0.4
```

The exact version and build information may differ.

### Check the JDK directly

If you know the installation directory, bypass `PATH` to verify the JDK:

```powershell
& "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\java.exe" -version
```

```powershell
& "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\javac.exe" -version
```

Replace the example directory with the actual installation path.

This direct check helps determine whether the JDK works even when Windows is using another Java installation.

---

## 3. Fix Java 8 Taking Priority

On some Windows computers, Oracle Java 8 is added to `PATH` through a directory such as:

```text
C:\Program Files (x86)\Common Files\Oracle\Java\java8path
```

This directory may take priority over a newer JDK.

### Step 1: Check which Java Windows is using

```powershell
where.exe java
```

Example:

```text
C:\Program Files (x86)\Common Files\Oracle\Java\java8path\java.exe
```

If this is the first result, Windows may be using Java 8 instead of JDK 25.

### Step 2: Set JAVA_HOME to JDK 25

Example installation path:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot
```

Set it in the current PowerShell session:

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
```

### Step 3: Put the JDK 25 bin directory first in PATH

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

This changes the environment of the current PowerShell session and its child processes.

It does not permanently modify the Windows system environment variables.

### Step 4: Verify the result

```powershell
java -version
```

```powershell
javac -version
```

```powershell
where.exe java
```

```powershell
where.exe javac
```

The JDK 25 executables should now be selected first.

Expected first Java path:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\java.exe
```

Expected first compiler path:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\javac.exe
```

### Important

If `where.exe javac` previously returned no results, it may mean that no JDK compiler directory was available through `PATH`.

It does not necessarily mean the JDK is missing.

Always check the actual installation directory before installing Java again.

---

## 4. Switch Java Versions

PowerShell can switch between installed JDK versions without permanently changing Windows environment variables.

This is useful when working on shared computers.

### Method A: Switch versions manually

#### Switch to JDK 25

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify:

```powershell
java -version
javac -version
```

#### Switch to JDK 21

If JDK 21 is installed at the example location below:

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-21"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify:

```powershell
java -version
javac -version
```

Replace the example path with the actual JDK 21 directory.

**Note:** Repeatedly prepending different JDK paths can leave several Java directories in the same `PATH`. For frequent switching, use Method B.

### Method B: Create reusable PowerShell switching commands

The following functions allow you to switch between JDK versions without manually rebuilding `PATH` every time.

They preserve the original PATH and replace the previous temporary JDK selection when switching again.

Run this block once in the current PowerShell session:

```powershell
$script:OriginalJavaPath = $env:Path

function Use-Jdk {
    param(
        [Parameter(Mandatory = $true)]
        [string]$Path
    )

    $jdkPath = (Resolve-Path $Path -ErrorAction Stop).Path
    $javaExe = Join-Path $jdkPath "bin\java.exe"
    $javacExe = Join-Path $jdkPath "bin\javac.exe"

    if (-not (Test-Path $javaExe) -or -not (Test-Path $javacExe)) {
        throw "The specified directory is not a valid JDK: $jdkPath"
    }

    $env:JAVA_HOME = $jdkPath
    $env:Path = "$jdkPath\bin;$script:OriginalJavaPath"

    Write-Host "Active JDK: $env:JAVA_HOME" -ForegroundColor Green

    java -version
    javac -version
}
```

#### Activate JDK 25

```powershell
Use-Jdk "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
```

#### Activate JDK 21

```powershell
Use-Jdk "C:\Program Files\Eclipse Adoptium\jdk-21"
```

Replace the example JDK 21 path with the actual installation directory.

#### Check the current selection

```powershell
$env:JAVA_HOME
```

```powershell
java -version
javac -version
```

**Important:** These functions exist only in the PowerShell session where they were defined. Closing PowerShell removes them.

To reuse them in future sessions, save the function definitions in a PowerShell profile or a script file stored in a persistent location.

### Method C: Set JAVA_HOME permanently

Use this method only when you want Windows to use a particular JDK by default.

Open the Windows Environment Variables interface:

1. Press `Win + R`.
2. Enter `SystemPropertiesAdvanced`.
3. Click **Environment Variables**.
4. Under User variables, create or edit `JAVA_HOME`.
5. Set its value to the actual JDK root directory.
6. Edit the User `Path` variable and add `%JAVA_HOME%\bin`.
7. Move that entry above conflicting Java entries where possible.
8. Confirm the changes and open a new PowerShell terminal.

Example `JAVA_HOME`:

```text
C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot
```

**Important:** Windows combines User and System environment variables. A conflicting System PATH entry can still take priority.

If Oracle Java 8 continues to take priority, inspect both User and System PATH entries. Do not delete Java 8 files or registry entries merely to change the default JDK.

On a shared computer, permanent changes may also be reverted by system-reset software.

---

## 5. Configure Visual Studio Code

VS Code can use a JDK for its Java language features independently of the Java version selected in the terminal.

### Step 1: Install the Java extensions

Open VS Code and press:

```text
Ctrl + Shift + X
```

Search for and install:

**Extension Pack for Java**

Official extension:

[Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

### Step 2: Configure the Java runtime

Press:

```text
Ctrl + Shift + P
```

Search for:

```text
Java: Configure Java Runtime
```

Inspect the JDK installations detected by VS Code.

If JDK 25 is missing, check the installation path and Java runtime settings.

### Step 3: Configure JDK 25 in settings.json

Open the Command Palette and search for:

```text
Preferences: Open User Settings (JSON)
```

Add or merge the following configuration:

```json
{
    "java.configuration.runtimes": [
        {
            "name": "JavaSE-25",
            "path": "C:\\Program Files\\Eclipse Adoptium\\jdk-25.0.4.101-hotspot",
            "default": true
        }
    ]
}
```

Use the actual JDK path installed on your computer.

If your settings file already contains other settings, do not replace the entire file. Merge the `java.configuration.runtimes` property with your existing configuration.

### Important: VS Code terminal versus Java extension

These settings serve different purposes:

- `JAVA_HOME` and `PATH` affect Java commands launched by the terminal.
- `java.configuration.runtimes` configures Java runtimes recognized by the Java extension.
- Gradle may use another JDK depending on its environment and project configuration.

Changing one setting does not automatically synchronize every Java environment.

### Shared computer note

If the computer resets after restarting, verify the JDK path and VS Code settings again when needed.

---

## 6. Configure Gradle Projects

Minecraft mods and plugins commonly use Gradle.

The JDK used by the terminal, Gradle, and the Minecraft test runtime may differ.

### Step 1: Activate the required JDK

For JDK 25:

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Use the actual path and the Java version required by your project.

### Step 2: Verify Java

```powershell
java -version
```

```powershell
javac -version
```

### Step 3: Check the Gradle runtime

From the project directory, run:

```powershell
.\gradlew.bat --version
```

This reports information about the Gradle Wrapper and the JVM it uses.

If the project does not contain `gradlew.bat`, check its documentation before attempting to build it.

### Step 4: Build the project

```powershell
.\gradlew.bat build
```

The Gradle Wrapper uses the version specified by the project.

### Step 5: Check the build output

For many Gradle projects, generated JAR files are located in:

```text
build\libs\
```

The exact output depends on the project configuration.

### Java compatibility notes

Not every Minecraft project requires JDK 25.

- Use the Java version required by the Minecraft version, mod loader, and build configuration.
- Minecraft versions and mod loaders can require different Java versions.
- An older project may require a particular Gradle version.
- The Java version used to run Gradle is not necessarily the same as the project's Java compilation target.
- A Gradle toolchain may select a different JDK from the terminal's `JAVA_HOME`.

Do not change a project's Java target or Gradle configuration just because a newer JDK is installed.

---

## 7. Portable JDK Setup

A portable JDK can be useful when working on shared Windows computers that reset after restarting.

Instead of relying on a system-wide installation, keep an extracted JDK on storage that persists between sessions, such as a USB drive.

### Step 1: Download the ZIP package

Open the official JDK 25 download page:

[Download Eclipse Temurin JDK 25](https://adoptium.net/temurin/releases/?version=25)

Select:

- Operating System: Windows
- Architecture: x64
- Package Type: JDK
- Version: 25
- Package: ZIP

### Step 2: Extract the archive

Extract the ZIP archive to a persistent location.

Example:

```text
E:\Java\jdk-25
```

Replace `E:` with your actual USB drive letter.

The JDK root should contain directories such as:

```text
bin\
conf\
include\
jmods\
lib\
```

The Java executables should be located at:

```text
E:\Java\jdk-25\bin\java.exe
```

```text
E:\Java\jdk-25\bin\javac.exe
```

If the archive extracts into a nested folder, use the actual directory containing `bin`.

### Step 3: Activate the portable JDK

```powershell
$env:JAVA_HOME = "E:\Java\jdk-25"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify:

```powershell
java -version
javac -version
```

### Important

A portable JDK is not automatically persistent just because it was extracted from a ZIP file.

Keep it on storage that survives the PC reset.

If your USB drive letter changes, update the path in your commands.

---

## 8. Troubleshooting

### Problem: Java reports version 8 instead of 25

Check the executable locations:

```powershell
where.exe java
```

```powershell
where.exe javac
```

Inspect the active environment variable:

```powershell
$env:JAVA_HOME
```

Set the correct JDK:

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify again:

```powershell
java -version
javac -version
```

If Java 8 is still selected, inspect the executable paths and confirm that the JDK 25 `bin` directory appears first.

### Problem: JAVA_HOME is empty

Set it for the current PowerShell session:

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
```

Check it:

```powershell
$env:JAVA_HOME
```

This change is temporary unless you configure the variable permanently.

### Problem: java is not recognized

Check that the JDK executable exists:

```powershell
Test-Path "$env:JAVA_HOME\bin\java.exe"
```

Then update PATH:

```powershell
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

Verify:

```powershell
java -version
```

### Problem: javac is not recognized

Check that the JDK compiler exists:

```powershell
Test-Path "$env:JAVA_HOME\bin\javac.exe"
```

Then verify:

```powershell
javac -version
```

If the file does not exist, confirm that you installed a JDK rather than a runtime-only package and that `JAVA_HOME` points to the JDK root directory.

### Problem: VS Code uses a different JDK

1. Open the Command Palette.
2. Run `Java: Configure Java Runtime`.
3. Check the configured JDK path.
4. Verify that the installation directory still exists.
5. Reload VS Code or restart the Java language server if necessary.

Also check whether the integrated terminal uses the expected `JAVA_HOME` and PATH.

### Problem: Gradle reports an incompatible Java version

Check the Gradle Wrapper:

```powershell
.\gradlew.bat --version
```

Review the project's:

- `gradle-wrapper.properties`
- Gradle build scripts
- Java toolchain configuration
- Minecraft and mod loader requirements

A newer JDK is not automatically compatible with an older Gradle release.

### Problem: Java settings disappear after restarting

Shared computers may use system-reset software that restores the original configuration.

Keep a copy of this guide and any required setup scripts in a persistent location.

After restarting, verify:

```powershell
java -version
```

```powershell
javac -version
```

If necessary, activate the JDK again using the temporary PowerShell commands.

---

## 9. Quick Command Reference

### Check the Java runtime

```powershell
java -version
```

### Check the Java compiler

```powershell
javac -version
```

### Check JAVA_HOME

```powershell
$env:JAVA_HOME
```

### Find Java executables in PATH

```powershell
where.exe java
```

```powershell
where.exe javac
```

### Find JDK 25 installations under Eclipse Adoptium

```powershell
Get-ChildItem "C:\Program Files\Eclipse Adoptium" -Recurse -Filter javac.exe -ErrorAction SilentlyContinue |
    Select-Object -ExpandProperty FullName
```

### Activate JDK 25 for the current PowerShell session

```powershell
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
```

### Check the Gradle runtime

```powershell
.\gradlew.bat --version
```

### Build a Gradle project

```powershell
.\gradlew.bat build
```

**Remember:** Replace example JDK paths with the actual installation directory on your computer.

---

## 10. Official Documentation

- [Eclipse Temurin JDK 25 Downloads](https://adoptium.net/temurin/releases/?version=25)
- [Eclipse Temurin Windows Installation](https://adoptium.net/installation/windows)
- [Java in Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Managing Java Projects in VS Code](https://code.visualstudio.com/docs/java/java-project)
- [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)

---

*Created as a practical reference for Java development on Windows, especially when working on shared computers with temporary environments.*

**End of Guide**
