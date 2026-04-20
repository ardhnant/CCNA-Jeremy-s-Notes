## 1 — What is Cisco IOS and CLI

- **Cisco IOS**: Cisco’s OS for routers/switches. Not Apple iOS.
    
- **CLI**: Command-line interface; primary tool for network engineers. GUIs exist (e.g., ASDM) but are not covered.
    

---

## 2 — How to connect to a Cisco device (console)

- **Local console** used for first-time device setup.
    
- Typical physical steps:
    
    1. Plug console cable into console port (RJ45 or USB-mini).
        
    2. Connect other end to serial port or USB-serial adapter on laptop.
    
    3. The name of the adapter is `Rollover Cable` where first end has a RJ45 connector and the other end is made of DB9 connector.
    
    4. You would need another cable to which adapt DB9 adapter to connect to USB port in a laptop
        
    5. Launch PuTTY (or Tera Term), ~`choose Serial, open session`~.
        

### Console ports & cables

|Console Port|Cable Type|Notes|
|--:|:--|:--|
|RJ45 console|**Rollover cable** (RJ45-to-DB9 wiring reversed)|Pins: 1↔8, 2↔7, 3↔6, 4↔5 … not an Ethernet crossover|
|USB console|**USB Mini-B**|Direct to laptop USB port|
|Laptop without serial|**USB-to-serial adapter**|Most modern laptops need this|

---

## 3 — Terminal settings (serial defaults — memorize)

- **Baud (speed)**: `9600` bps
    
- **Data bits**: `8`
    
- **Parity**: `none`
    
- **Stop bits**: `1`
    
- **Flow control**: `none`  
    (Shown as `9600 8 N 1 flow none` — exam favorite)
    

---

## 4 — Basic CLI modes & prompts

|Mode|Prompt|Purpose|
|---|---|---|
|User EXEC|`Router>`|Limited, read-only|
|Privileged EXEC|`Router#`|Full visibility, maintenance|
|Global Config|`Router(config)#`|Make configuration changes|

Common transitions:

- `enable` → privileged exec
    
- `configure terminal` (`conf t`) → global config
    
- `exit` → step back / logout
    

---

## 5 — CLI features & shortcuts (very CCNA-useful)

- **Tab**: auto-complete commands.
    
- **Abbreviations**: shortest unique prefix works (`en` → `enable`).
    
- **`?`**: context-sensitive help. `e?` shows commands starting with `e`.
    
- **`do`** (in config mode): run exec commands, e.g., `do show running-config`.
    
- `Ctrl+C` / escape sequences: interrupt operations (platform-dependent).
    

---

## 6 — Basic configuration workflow

1. Connect via console.
    
2. Press Enter until prompt appears.
    
3. `enable` → `conf t`.
    
4. Make config changes.
    
5. Save config (`copy run start` or `write`).
    

---

## 7 — Passwords & security (CCNA focus)

- `enable password <pwd>`
    
    - Older, stored in clear-text unless `service password-encryption` applied.
        
- `service password-encryption`
    
    - Applies weak Cisco type-7 obfuscation to displayed passwords. Not strong.
        
- `enable secret <pwd>`
    
    - Preferred: stored as MD5 (type 5) hash; takes precedence over `enable password`.
        
- Passwords are case-sensitive — Caps Lock is a common cause of login failure.
    

Notes:

- `service password-encryption` only changes display, not the actual password.
    
- If both `enable secret` and `enable password` are present, **enable secret wins**.
    

---

## 8 — Viewing and saving configs

- Show running config: `show running-config` (`sh run`) — active config.
    
- Show startup config: `show startup-config` (`sh start`) — loaded at boot.
    
- Save running → startup:
    
    - `write`
        
    - `write memory`
        
    - `copy running-config startup-config` (explicit & clear)
        

If `show startup-config` reports "not present," device will boot defaults on reload.

---

## 9 — Undoing commands

- Prefix with `no` in config mode to remove a previously configured command.
    
    - Example: `no service password-encryption`.
        

Behavior:

- Disabling `service password-encryption` does not decrypt already-encrypted strings; future passwords won’t be encrypted.
    

---

## 10 — Quick command reference

| Task                                    | Command                              |
| --------------------------------------- | ------------------------------------ |
| Enter privileged exec                   | `enable` (`en`)                      |
| Enter global config                     | `configure terminal` (`conf t`)      |
| Set enable password                     | `enable password <pwd>`              |
| Set secure enable secret                | `enable secret <pwd>`                |
| Enable weak encryption for display      | `service password-encryption`        |
| Show running config                     | `show running-config`                |
| Show startup config                     | `show startup-config`                |
| Save running to startup                 | `copy running-config startup-config` |
| Remove config line                      | `no <command>`                       |
| Run exec command from config mode       | `do show running-config`             |
| Configuring Hostnames                   | `hostname <host name>`               |
| Knowing details about the switch/router | `show version`                       |


---

## 11 — Common pitfalls & exam tips

- Memorize serial defaults: `9600, 8, N, 1, no flow`.
    
- Know `enable password` vs `enable secret` vs `service password-encryption`.
    
- Save configs or you’ll lose changes after reload.
    
- Shortcuts are allowed but know full forms for clarity.
    
- Caps Lock causes failed logins.
    

---

## 12 — Sample mini-lab (practice checklist)

1. Console connect to device in Packet Tracer.
    
2. `enable` → `conf t`.
    
3. `enable secret MyPass123`.
    
4. `service password-encryption`.
    
5. `do show running-config` and verify outputs.
    
6. `copy running-config startup-config`.
    
7. Practice `no` to remove a setting and observe effects.
    

---

## 13 — Quiz answers (from lesson)

1. RJ45 console uses a **rollover cable**. (A)
    
2. `enable` password not accepted — check **Caps Lock**. (C)
    
3. Most secure: **enable secret**. (A)
    
4. If both set, you must enter **enable secret** only. (C)
    
5. `conf t` full form: **configure terminal**. (B)
    

