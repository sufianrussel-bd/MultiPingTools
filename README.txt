===============================================================================
  MULTI PING TOOLS   v1.4
  Author       : Smart Technologies (BD) Ltd.
  Team         : Enterprise Service (Huawei Team)
  License      : Open Source
  Update Date  : 21 September 2026
  Supported    : Windows 10 / 11   and   Windows Server 2016 / 2019 / 2022
===============================================================================


WHAT'S NEW IN v1.4
------------------
  [+] TRULY SELF-CONTAINED PORTABLE BUILD
      MAKE_PORTABLE.bat bundles every dependency - the Python runtime,
      tkinter, the Tcl/Tk GUI files - into one folder. Python does NOT
      need to be installed on the PC that runs it.
  [+] ONE-CLICK UPDATE
      New "Update" button inside the program. It compares your version
      against a folder you configure (a file share, mapped drive or USB)
      and installs a newer build with one click - backing up your current
      folder first. Completely OFFLINE: no internet, no vendor server.
  [+] UPDATE.bat   - the updater itself, also runnable on its own
  [+] PUBLISH.bat  - for you as maintainer: push a build to the team share
  [+] version.txt  - stamped into every build automatically
  [+] About window now shows Supported OS and current update status
  [+] Numeric version comparison, so 1.10 correctly ranks above 1.9
      and a lower version on the share can never cause a downgrade


WHAT WAS ADDED IN v1.3
------------------
  [FIX] START SELECTED ROW now works WHILE AUTO UPDATE / the continuous
        scan is running. Previously the button was greyed out.
        You can now run the full continuous scan and, at the same time,
        pull one problem device out and watch it separately.
  [+] Right-click > "Stop this row"  - stop one row job without stopping
      the full scan
  [+] Stop All  - stops every running job
  [+] A row already being pinged cannot be started twice (no duplicates)
  [+] Rows now have a permanent internal ID, so sorting a column during a
      live scan can no longer put results on the wrong host
  [+] Full security hardening - see SECURITY.txt
  [+] New status: BAD TARGET (red) for entries rejected by the safety check


WHAT WAS ADDED IN v1.2
----------------------
  [-] "Packets" input REMOVED
  [+] CONTINUOUS PING - pings cycle after cycle until you press Stop
  [+] AUTO UPDATE button - toggle the continuous loop ON / OFF
  [+] Interval (sec) - idle gap between cycles
  [+] Sent / Recv / Loss % are CUMULATIVE across all cycles
  [+] Cycles counter, Reset Stats button, flapping detection


WHAT WAS ADDED IN v1.1
----------------------
  [+] Renamed to "Multi Ping Tools"
  [+] START SELECTED ROW, double-click a row, right-click menu
  [+] Total Ping Sent counter, Sent / Recv columns
  [+] Import CSV only (SL, Host Name, IP Address)
  [+] About window, Delete Row button
  [-] Excel export REMOVED - Export CSV only (opens fine in Excel anyway)


COLOUR CODE
-----------
  GREEN   REACHABLE     up now, 0% cumulative loss
  YELLOW  PARTIAL       up now, but lost packets in earlier cycles
                        = FLAPPING / lossy link   <-- watch this one
  RED     TIMEOUT / UNREACHABLE / DNS FAIL / BAD TARGET
  GREY    PENDING       not pinged yet


===============================================================================
  RUNNING TWO JOBS AT ONCE   (the v1.3 fix)
===============================================================================

  1. Import your CSV, press START ALL.
     The continuous scan begins and keeps cycling.

  2. You spot a device flapping yellow. Click that row and press
     START SELECTED ROW  (or just double-click it).
     -> It starts its OWN continuous job, running alongside the full scan.

  3. Right-click that row > "Stop this row" when you are done.
     The full scan carries on untouched.

  4. "Stop All" stops everything.

  Notes
  -----
  * A row that is already part of a running job cannot be started again -
    the tool tells you instead of creating a duplicate.
  * START ALL is greyed out only while a full scan is running. START
    SELECTED ROW is ALWAYS available.
  * You cannot delete a row while it is being pinged. Stop it first.


===============================================================================
  HOW CONTINUOUS PING WORKS
===============================================================================

  Press START ALL. The tool then repeats forever:

      cycle 1 : ping every host once  -> update table
      wait <Interval> seconds
      cycle 2 : ping every host once  -> update table
      ... until you press Stop All

  Sent / Recv / Loss % keep adding up over every cycle, so after an hour
  you can see exactly how many packets a link dropped - far more useful
  than a one-shot 4-packet test.

  Status bar:
     Hosts N | UP n | PARTIAL n | DOWN n | Cycles: n | Total Ping Sent: n

  Reset Stats   zeroes all counters but keeps your host list loaded


===============================================================================
  HOW TO RUN  -  PICK ONE
===============================================================================

  Which one do I want?

    Just want to use it now, Python is installed   ->  RUN.bat
    Want ONE portable .exe file to copy around     ->  MAKE_EXE_1CLICK.bat
    Want a fast portable FOLDER for a USB stick    ->  MAKE_PORTABLE.bat

  The two MAKE_* scripts need Python ONCE, on the PC where you build.
  The thing they produce does NOT need Python anywhere else.


OPTION A - RUN.bat            (instant, no build)
--------------------------------------------------
    Double-click  RUN.bat

    Starts the tool straight away. Nothing is installed or compiled.
    Requires Python on this PC. This is the safest option - you are
    running the readable source, not a compiled binary.

    Equivalent command line:
        python multi_ping_tools.py

    No pip install needed. The tool uses only the Python standard
    library - zero external dependencies.


OPTION B - MAKE_EXE_1CLICK.bat        (one single portable .exe)
-----------------------------------------------------------------
    Double-click  MAKE_EXE_1CLICK.bat
    Wait 1-3 minutes.

    Result:   MultiPingTools.exe     (right here in this folder)

    That ONE file is completely portable:
       - copy it to a USB stick, a shared drive, a jump server
       - Python is NOT needed on the target PC
       - nothing is installed, no registry entry, no admin rights
       - to remove it, just delete the file

    The script does everything itself: finds Python, runs the security
    self-test, installs PyInstaller if missing, builds, cleans up, and
    opens the folder with the EXE selected.

    Trade-off: a --onefile EXE unpacks itself into a temp folder on every
    launch, so it starts ~1-2 seconds slower and is the variant most
    likely to be flagged by antivirus. If that bothers you, use Option C.


OPTION C - MAKE_PORTABLE.bat    <-- RECOMMENDED for your team
--------------------------------------------------------------------------
    Double-click  MAKE_PORTABLE.bat
    Wait 1-3 minutes.

    Result:   MultiPingTools_Portable\        (a folder)
              MultiPingTools_Portable.zip     (the same, zipped)

    EVERYTHING IS IN THAT ONE FOLDER
       The Python runtime, tkinter, the Tcl/Tk GUI files, the app - all
       bundled. PYTHON DOES NOT NEED TO BE INSTALLED on the PC that runs
       it. Nothing is installed, no admin rights, no registry entries.
       To remove it, delete the folder.

    Copy the whole FOLDER to a USB stick or a share. On any Windows PC
    open it and double-click MultiPingTools.exe inside.

    Supported there:
       Windows 10, Windows 11
       Windows Server 2016, 2019, 2022      (64-bit)

       A PyInstaller build targets the Windows version you built it on
       and newer, so build on Windows 10 / Server 2016 or later. The
       script warns you if the build machine is older.

    Why this is usually the better choice than Option B:
       - starts instantly (no self-extraction on each launch)
       - antivirus rarely complains about it
       - supports the one-click Update button (see below)

    Keep the .exe together with the files next to it - those are its
    runtime. Do not move the .exe out of the folder on its own.

    Copied in automatically: README.txt, SECURITY.txt, sample_hosts.csv,
    UPDATE.bat, version.txt and an empty update_source.txt.


  Manual build command, if you prefer doing it yourself:
        pip install pyinstaller
        pyinstaller --onefile --windowed --name MultiPingTools multi_ping_tools.py

  Build it YOURSELF. Do not accept a prebuilt EXE from anyone, since you
  cannot verify what is inside a compiled binary. The .py source in this
  folder is the authoritative artifact.


VERIFY THE TOOL
---------------
    python selftest.py

    Runs every functional and security check and prints pass/fail for
    each one. Exit code 0 means everything passed.

    Both MAKE_* scripts run this automatically and REFUSE to build if
    anything fails, so a broken build never reaches your team.


===============================================================================
  ONE-CLICK UPDATE   (offline - no internet)
===============================================================================

  The idea: you publish a new build to a shared folder once; every
  engineer then just presses the "Update" button inside the program.

  Nothing goes over the internet. There is no vendor update server.
  It is a plain file copy from a folder that YOU choose.


  SETUP - YOU, ONCE  (the maintainer)
  ------------------------------------
    1. Build it:        double-click  MAKE_PORTABLE.bat
    2. Publish it:      double-click  PUBLISH.bat
                        type the shared folder when it asks, e.g.
                            \\fileserver\tools\MultiPingTools
    3. Hand each engineer a copy of MultiPingTools_Portable\ whose
       update_source.txt already contains that same path.

    Make the share READ-ONLY for ordinary users. Only the people who
    publish builds should have write access to it.


  DAY TO DAY - YOUR TEAM
  -----------------------
    Press the  Update  button in the toolbar.

      - already current    -> "You are up to date."
      - newer available    -> shows old and new version, asks to confirm,
                              then closes, backs up, copies, relaunches
      - not configured     -> tells them exactly which file to fill in
      - share unreachable  -> tells them to check VPN / mapped drive

    They can also double-click UPDATE.bat directly - same thing.


  RELEASING A NEW VERSION LATER
  ------------------------------
    1. Edit multi_ping_tools.py, change:   VERSION = "1.5"
    2. Run MAKE_PORTABLE.bat   (version.txt is stamped automatically)
    3. Run PUBLISH.bat to the same share
    Done. Everyone's Update button now offers 1.5.


  update_source.txt FORMAT
  -------------------------
    One folder path on a line. Lines starting with # are comments.

        # Multi Ping Tools - offline update source
        \\fileserver\tools\MultiPingTools

    Other valid examples:
        D:\Tools\MultiPingTools
        E:\MultiPingTools            (USB stick)

    Leave it empty to DISABLE updating entirely. The button then only
    reports "not configured" - everything else in the tool still works.


  SAFETY BUILT IN
  ----------------
    * Your folder is backed up to _backup_<version>_<timestamp> BEFORE
      anything is overwritten. A bad update is reversible - copy the
      files back.
    * Version comparison is numeric: 1.10 ranks above 1.9, and an older
      version on the share can never silently downgrade you.
    * update_source.txt is never overwritten by an update, so each PC
      keeps its own configuration.
    * The updater waits for the program to close before touching files.
    * If the folder is unreachable or incomplete, nothing is changed.

    Read section 3A of SECURITY.txt before rolling this out - it explains
    the one risk you are accepting (whoever can write to the share
    controls what your team runs) and how to control it.


IF THE BUILD PC HAS NO INTERNET
-------------------------------
    PyInstaller has to be downloaded once. On an offline machine:
       1. On a PC with internet:   pip download pyinstaller -d pyi_pkgs
       2. Copy the pyi_pkgs folder across
       3. On the offline PC:       pip install --no-index --find-links pyi_pkgs pyinstaller
       4. Then run MAKE_EXE_1CLICK.bat

    Behind a company proxy:
       pip install --proxy http://user:pass@proxy:port pyinstaller


===============================================================================
  CSV FORMAT   (import is CSV only)
===============================================================================

    SL,Host Name,IP Address
    1,SBC-A01-Spine-CE8861-01,10.10.1.1
    2,SBC-A01-BorderLeaf-01,10.10.1.2
    3,USG6680E-HQ-01,10.20.1.1

  The header row is detected and skipped automatically.
  Separator can be comma, semicolon, tab or pipe - auto-detected.

  These variations also work:

    1,Core-SW-01,10.10.1.1          SL, host, IP        (standard)
    Border-Leaf-02,10.10.1.2        host, IP            (SL auto-numbered)
    10.30.1.1                       IP only
    4,10.40.1.1,Spine-CE8861        IP in the middle
    google.com                      hostname / FQDN     (DNS ping)

  Lines starting with # are ignored.
  sample_hosts.csv (200 rows) is included for testing.

  Rows whose target is not a valid host name or IP address are REJECTED
  and counted. The tool tells you how many. This is a safety feature - it
  blocks malicious CSV entries such as "-t" or "8.8.8.8 & calc".
  Limits: 8 MB file, 5000 rows.


===============================================================================
  BUTTONS AND SETTINGS
===============================================================================

  Import CSV            load host list
  Add Row               add one host manually
  Delete Row            remove selected row(s)  (must not be pinging)
  Clear                 empty the table
  Reset Stats           zero the counters, keep the hosts
  START ALL             continuous ping, all rows
  START SELECTED ROW    continuous ping, highlighted row(s) only
                        - works during a full scan
  Stop All              stop every running job
  AUTO UPDATE: ON/OFF   ON  = keep cycling forever  (default)
                        OFF = run one single pass, then stop
  Export CSV            save result (header + rows + SUMMARY block)
  Update                check the configured folder for a newer build
                        (offline - see the ONE-CLICK UPDATE section)
  About                 version / OS support / update status / license

  Right-click a row     Ping this row / Stop this row / Delete row
  Double-click a row    ping just that row
  Click a column header sort by it (safe during a live scan)

  Settings
     Timeout (ms)    wait per packet                 default 1000
     Threads         hosts pinged simultaneously     default 60
     Interval (sec)  idle gap between cycles         default 2  (0 = flat out)
     Filter          ALL / REACHABLE / DOWN ONLY / PARTIAL

  There is no "Packets" setting - the tool sends 1 packet per host per
  cycle and keeps cycling, which is what continuous ping means.

  The exported CSV contains:
     - a title block (tool name, version, author, generated timestamp)
     - the full result table (cumulative Sent / Recv / Loss %)
     - a SUMMARY block: Total Hosts / UP / PARTIAL / DOWN / Total Ping Sent


===============================================================================
  SECURITY  (short version - full audit in SECURITY.txt)
===============================================================================

  * Standard library only. No third-party package, no installer, no DLL.
  * NO network egress. The tool never opens a socket, never contacts any
    server, never downloads anything. Nothing leaves your computer.
  * "AUTO UPDATE" refreshes the ping RESULTS on screen. It does NOT fetch
    software - that is the separate "Update" button.
  * The "Update" button copies files from a folder YOU configure. It is
    offline: no internet, no vendor server, and it is inert until you
    fill in update_source.txt. Read SECURITY.txt section 3A.
  * Targets are validated against a strict allowlist before reaching ping,
    which blocks command-line argument injection from a malicious CSV.
  * ping runs with shell=False and a fixed argument list - shell
    metacharacters can never be interpreted.
  * Exported CSV values are neutralised against Excel formula / DDE
    injection (CWE-1236).
  * No eval / exec / pickle / os.system / temp files / registry writes /
    persistence / telemetry.
  * Runs as a normal user. No admin rights needed.

  ANTIVIRUS: a PyInstaller EXE may be flagged as a false positive because
  it unpacks itself at run time. That is expected. Prefer Option A, or
  whitelist the EXE you built yourself. Do not disable antivirus.


===============================================================================
  NOTES
===============================================================================

  * "Destination host unreachable" is handled correctly. Windows ping
    reports "0% loss" in that case - a naive tool would show GREEN.
    This tool checks for it first and correctly shows RED.

  * PARTIAL (yellow) is the most useful state for an engineer: the link is
    up right now but has dropped packets in earlier cycles. That is a
    flapping / lossy link - catch these before they become outages.

  * Interval 0 with 200 hosts means continuous ping with no pause. Fine on
    a LAN, but it can look like a scan to an IPS. On production networks
    keep Interval at 2 seconds or more.

  * Some networks block ICMP. A RED row does not always mean the device is
    down - ping may simply be filtered. Verify before raising an incident.

  * Keep Threads at or below 100 on production networks. Higher values can
    trigger ICMP rate limiting on firewalls and produce false RED results.

  * Windows Firewall may prompt on first run - allow it.


===============================================================================
  FILES
===============================================================================

  multi_ping_tools.py    main application (single file, readable, editable)

  RUN.bat                run it now, no build            <- easiest
  MAKE_EXE_1CLICK.bat    build ONE portable .exe         <- one click
  MAKE_PORTABLE.bat      build a portable FOLDER         <- recommended
  build_exe.bat          plain/manual build script

  UPDATE.bat             one-click offline update (ships in the build)
  PUBLISH.bat            maintainer: push a build to the team share

  README.txt             this file
  SECURITY.txt           full security review, findings and residual risk
  selftest.py            automated functional + security test suite
  sample_hosts.csv       200-row example CSV

  Created automatically by the build:
  version.txt            the version of that build
  update_source.txt      where the Update button looks (you fill it in)

===============================================================================
  Free and open source. Use, modify and distribute freely.
  Provided as-is, without warranty of any kind.
===============================================================================
