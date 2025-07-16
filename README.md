# **Proxmox VE: Enhanced Tag-Based VM Organization (Oz Fork)**  
**Version:** 1.1  
**Maintainer:** Oz Abramovich  
**Original Author:** Gabriel Adams  
**Date:** June 2025  

---

## 🔄 **About This Fork**  

This is a maintained and slightly modified version of the original repository created by **Gabriel Adams**.  
It introduces some fixes and improvements, including bug fixes that occur when clicking on folders.

Credit and appreciation go to Gabriel for the original concept and implementation.  
You can find the original project here: [Gabriel’s Repo](https://github.com/gradams42/ProxmoxUpgrades)

---

## **Overview**  

This enhancement improves the **Proxmox Virtual Environment (Proxmox VE)** UI by introducing an **automated, structured folder system** based on **VM and container tags**.  

Instead of a flat resource list, this system dynamically organizes VMs into a **nested directory structure** using their assigned tags, allowing administrators to manage **large-scale Proxmox environments** more efficiently.

---

![Demonstration](Organized_Node.png)

---

## **Key Features**  

### ✅ Tag-Based Folder Organization  
- The **first tag** defines the parent folder.  
- Additional tags create **nested subfolders**.  
- Tags should be prefixed (`A-`, `B-`, etc.) for consistent sorting and nesting.

Example:
- A-Production → B-Database  
- A-Production → B-WWW  
- A-Test → B-WWW  

This helps maintain the correct hierarchy (`Production → Database`) instead of something unintended like (`Database → Production`).

### 🔄 Enhanced Automatic Updates (New!)
- **Real-time updates**: No more manual refreshing needed! The folder structure updates automatically when you make changes to VM/container tags.
- **Smart update intervals**: After a tag change, the system updates every 1 second for 10 seconds, then returns to normal 3-second intervals.
- **Immediate feedback**: Changes are reflected in the tree view within 1-3 seconds of making them.
- **API monitoring**: The system automatically detects API calls that modify tags and triggers immediate updates.
- **Visual feedback**: Subtle visual indicators show when data has been refreshed.

### ⚙️ Configurable Update System
The automatic update system can be customized through JavaScript:

```javascript
// Get current configuration
let config = Proxmox.Utils.getAutoUpdateConfig();

// Customize settings
Proxmox.Utils.setAutoUpdateConfig({
    enabled: true,          // Enable/disable enhanced updates
    baseInterval: 3000,     // Normal update interval (3 seconds)
    fastInterval: 1000,     // Fast update after changes (1 second)
    fastUpdateDuration: 10000, // How long to use fast updates (10 seconds)
    visualFeedback: true    // Enable visual feedback
});
```

### 🧹 Automatic Cleanup  
- If a VM loses all tags, it moves back to the default folder.  
- Empty folders are automatically removed.

---

## **Benefits Overview**  

| Feature            | Before                | After                           |
|--------------------|------------------------|----------------------------------|
| VM Organization     | Flat list of VMs       | Hierarchical folders via tags    |
| Folder Naming       | Unstructured           | Uses tag names                   |
| Tag Consistency     | Inconsistent           | Enforced structure               |

---

## 🔧 **Installation & Setup**  

### Step 1: Backup the Existing UI Script  

```bash
cp /usr/share/pve-manager/js/pvemanagerlib.js /usr/share/pve-manager/js/pvemanagerlib.js.bak
cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak
```

---

### Step 2: Replace `pvemanagerlib.js` and `proxmoxlib.js` 

```bash
git clone https://github.com/Dark-Avenger-Reborn/Proxmox-Folder-Mod
cd Proxmox-Folder-Mod

cp pvemanagerlib.js /usr/share/pve-manager/js/
cp proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/

chmod 644 /usr/share/pve-manager/js/pvemanagerlib.js
chmod 644 /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```

---


### Step 4: Restart the Proxmox Web Interface  

```bash
systemctl restart pveproxy
systemctl restart pvedaemon
```

---

### Step 5: Force Refresh the Web UI  

Use:  
**`CTRL + Shift + R`**  
to clear your browser’s cache and reload the interface.

---

## 🛠️ Future Plans  

- Improve tag sorting logic (avoid alphabetical override).  
- Make real-time updates even smoother.  
- Better backend-tag communication for true hierarchy support.  
- Seamless integration with Proxmox’s existing tag system and API.

---

## 🙏 Credits  

- **Original Author:** [Gabriel Adams](https://github.com/gradams42)  
- **Maintained Fork by:** Oz Abramovich  
- **Community:** Built for the Open Source Proxmox Community  

---
