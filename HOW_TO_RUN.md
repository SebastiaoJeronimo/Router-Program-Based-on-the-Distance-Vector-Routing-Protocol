# How to run the router

This is a Java Swing program. The main class is `router.Router`. NetBeans is optional.

JDK 25 is already installed at `C:\Program Files\jdk-25.0.4.1`, but `java` and `javac` are not on the PATH. Use the full paths below. `java` only runs compiled `.class` files, so the `javac` step has to succeed first.

The source lives in:

`router/router/src`

## 1. Check Java

Open PowerShell and run:

```powershell
& "C:\Program Files\jdk-25.0.4.1\bin\java.exe" -version
& "C:\Program Files\jdk-25.0.4.1\bin\javac.exe" -version
```

Both should report version **25**.

## 2. Compile and start one window

From the project folder:

```powershell
cd "router\router\src"
& "C:\Program Files\jdk-25.0.4.1\bin\javac.exe" --release 21 router\*.java
& "C:\Program Files\jdk-25.0.4.1\bin\java.exe" router.Router
```

A window opens. Leave that PowerShell window open. Closing it stops the router.

To start another router, open a **second** PowerShell window and run only:

```powershell
cd "router\router\src"
& "C:\Program Files\jdk-25.0.4.1\bin\java.exe" router.Router
```

Compile once. Each extra window is another `java.exe router.Router`. The sources are already compiled in `router\router\src\router`.

## 3. Clean the class files

Compiling writes `*.class` files next to the `.java` files, in `router\router\src\router`. To delete only those compiled files, from the `src` folder:

```powershell
cd "router\router\src"
Remove-Item router\*.class
```

The `.java` sources stay. Compile again before the next run:

```powershell
& "C:\Program Files\jdk-25.0.4.1\bin\javac.exe" --release 21 router\*.java
```

## 4. Start two routers on this PC

Each router needs its own name and UDP port. A name is one uppercase letter (`A` to `Z`).

1. In the first window, click **Active**. It keeps the defaults: name `A`, port `20000`. The IP box changes to this computer’s address.
2. In the second window, click **Active**. Port `20000` is already taken, so this window becomes name `B` on port `20001`.
3. Go back to router **A**. The neighbour fields should already be name `B`, distance `1`, and port `20001`. Click **Add Neig**.
4. Router **A** sends a HELLO. Router **B** adds **A** on its own. Both neighbour tables should show the other router.

If the neighbour fields on **A** are wrong, set them by hand before **Add Neig**:

| Field | Router A | Router B |
| --- | --- | --- |
| Name | A | B |
| Port | 20000 | 20001 |
| Neighbour name | B | A |
| Neighbour port | 20001 | 20000 |
| Neighbour distance | 1 | 1 |
| Neighbour IP | same IP shown in the other window | same IP shown in the other window |

Click **Active** before **Add Neig**. Buttons do nothing useful until the router is active.

## 5. What the window does

- **Active** starts or stops the router. Name, IP, port, ROUTE period, Split Horizon, and Hold down lock while it is on.
- **Add Neig** / **Rem. Neig** / **Mod. Neig** change the neighbour list. Distance must be from 1 to 20.
- **Send if changes** is disabled in this version of the code.
- **ROUTE Period** defaults to `4` seconds. **Split Horizon** starts checked. **Hold down** starts off, with time `14`.
- The routing table columns are Destination, Router, Distance, and Holddown.
- **Send** sends the message in **Msg** to the router named in **Dest**.
- **Clear** clears the log. The same text is also printed in the PowerShell window.

## 6. Run it from NetBeans

1. Install JDK 21 and [Apache NetBeans](https://netbeans.apache.org/).
2. **File → Open Project** and choose the folder `router/router` (the one that contains `nbproject`).
3. Right-click the project and choose **Run**. The main class is already `router.Router`.
4. Run the project again, or use **Run → Run Project** a second time, to open another router window.

`Router.form` is only for the NetBeans form designer. You do not need it to compile or run.

## 7. Common problems

- **`java` or `javac` is not recognized.** Those commands are not on the PATH. Use the full paths in the sections above.
- **`Could not find or load main class router.Router`.** The `.java` files were not compiled. From `router\router\src`, run the `javac.exe` command, then `java.exe`. `Router.class` must exist in the `router` folder.
- **`Router is not active`.** Click **Active** first.
- **`Invalid name`.** Use one uppercase letter, such as `A` or `B`.
- **Second window did not become B.** Port `20000` was free, so both tried to be A. Stop one, set its name to `B` and its port to `20001`, then click **Active**.
- **No HELLO on the other window.** Both routers must be active, and the neighbour IP and port must match the other window exactly.
