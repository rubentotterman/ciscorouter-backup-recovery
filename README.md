# ciscorouter-backup-recovery

# Cisco Device Management Lab


![Alt Text](15 Cisco Device Management.JPG)


This repository contains lab exercises and configuration steps for managing Cisco network devices, including routers and switches. It focuses on critical skills such as factory reset, password recovery, configuration backup, IOS image backup, and software upgrades.

---

## 🧪 Lab Overview

In this lab, you will:

- Perform a factory reset on a Cisco router
- Recover device passwords
- Backup and restore router configurations
- Backup and recover IOS system images
- Upgrade switch IOS software

---

## 🔁 Factory Reset (Router R1)

1. **View the current configuration**:
   ```
   show run
   ```

2. **Erase the startup configuration**:
   ```
   write erase
   reload
   ```

3. **Exit the Setup Wizard and verify configs are cleared**:
   ```
   show run
   show start
   ```

4. **Reapply configuration**:
   ```
   configure terminal
   hostname R1
   interface GigabitEthernet0/0
   ip address 10.10.10.1 255.255.255.0
   duplex auto
   speed auto
   no shutdown
   line con 0
   exec-timeout 30 0
   end
   copy run start
   ```

---

## 🔐 Password Recovery

1. **Set config register to enter ROMMON**:
   ```
   config-register 0x2120
   reload
   ```

2. **In ROMMON mode**:
   ```
   confreg 0x2142
   reset
   ```

3. **Exit Setup Wizard**:
   ```
   no
   ```

4. **Verify configurations**:
   ```
   show run
   show start
   ```

5. **Restore configuration**:
   ```
   copy start run
   ```

6. **Enable interface**:
   ```
   interface GigabitEthernet0/0
   no shutdown
   ```

7. **Remove the enable secret**:
   ```
   no enable secret
   ```

8. **Restore config register and save config**:
   ```
   config-register 0x2102
   end
   copy run start
   reload
   ```

---

## 💾 Configuration Backup

- **Backup running config to flash**:
  ```
  copy run flash:
  Destination filename [running-config]? Backup-1
  ```

- **Backup startup config to TFTP**:
  ```
  copy start tftp:
  Address or name of remote host []? 10.10.10.10
  Destination filename [R1-confg]? Backup-2
  ```

---

## 📦 IOS System Image Backup & Recovery

1. **Backup IOS to TFTP**:
   ```
   show flash
   copy flash: tftp:
   Source filename []? c2900-universalk9-mz.SPA.151-4.M4.bin
   Address or name of remote host []? 10.10.10.10
   ```

2. **Delete IOS from flash and reload**:
   ```
   delete flash:c2900-universalk9-mz.SPA.151-4.M4.bin
   reload
   ```

3. **In ROMMON, set variables for `tftpdnld`**:
   ```
   IP_ADDRESS=10.10.10.1
   IP_SUBNET_MASK=255.255.255.0
   DEFAULT_GATEWAY=10.10.10.1
   TFTP_SERVER=10.10.10.10
   TFTP_FILE=c2900-universalk9-mz.SPA.151-4.M4.bin
   tftpdnld
   ```

4. **Confirm and start recovery**:
   ```
   y
   ```

5. **After recovery completes**:
   ```
   reset
   ```

---

## 🚀 Switch IOS Upgrade (SW1)

1. **Verify current IOS version**:
   ```
   show version
   ```

2. **Transfer new IOS from TFTP to flash**:
   ```
   copy tftp: flash:
   Address or name of remote host []? 10.10.10.10
   Source filename []? c2960-lanbasek9-mz.150-2.SE4.bin
   ```

3. **Set boot preference and save config**:
   ```
   config terminal
   boot system c2960-lanbasek9-mz.150-2.SE4.bin
   end
   copy run start
   ```

4. **Reload switch and verify version**:
   ```
   reload
   show version
   ```

---

## 📂 Files

- `15 Cisco Device Management Answer Key v2.docx` — Includes detailed instructions and expected output for each lab step.

---

## 🛠 Requirements

- Cisco router (e.g., 2900 series)
- Cisco switch (e.g., 2960 series)
- TFTP server on 10.10.10.10
- Console terminal (e.g., PuTTY, Tera Term)
- Cisco Packet Tracer (for simulated lab)

---

## 📚 References

- [Cisco ROMMON Recovery Documentation](http://www.cisco.com/c/en/us/td/docs/routers/access/1900/software/configuration/guide/Software_Configuration/appendixCrommon.html)

---

