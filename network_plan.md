I had deepseek help me with the plan.

---

#̶#̶#̶ *̶*̶1̶.̶ N̶e̶t̶w̶o̶r̶k̶ C̶o̶r̶e̶:̶ p̶f̶S̶e̶n̶s̶e̶ &̶ I̶S̶C̶ D̶H̶C̶P̶*̶*̶
-̶ *̶*̶D̶e̶p̶l̶o̶y̶m̶e̶n̶t̶:̶*̶*̶  
  -̶ *̶*̶p̶f̶S̶e̶n̶s̶e̶*̶*̶:̶ D̶e̶d̶i̶c̶a̶t̶e̶d̶ V̶M̶ (̶s̶m̶a̶l̶l̶ i̶n̶s̶t̶a̶n̶c̶e̶,̶ e̶.̶g̶.̶,̶ A̶z̶u̶r̶e̶ B̶2̶s̶)̶.̶  
    -̶ *̶*̶W̶h̶y̶ s̶e̶p̶a̶r̶a̶t̶e̶?̶*̶*̶ S̶e̶c̶u̶r̶i̶t̶y̶ +̶ s̶t̶a̶b̶i̶l̶i̶t̶y̶ (̶r̶o̶u̶t̶e̶r̶/̶f̶i̶r̶e̶w̶a̶l̶l̶ s̶h̶o̶u̶l̶d̶ s̶t̶a̶y̶ i̶s̶o̶l̶a̶t̶e̶d̶)̶.̶  
    -̶ C̶o̶n̶f̶i̶g̶u̶r̶e̶ W̶A̶N̶ (̶p̶u̶b̶l̶i̶c̶ I̶P̶)̶ a̶n̶d̶ L̶A̶N̶ (̶p̶r̶i̶v̶a̶t̶e̶ s̶u̶b̶n̶e̶t̶)̶ i̶n̶t̶e̶r̶f̶a̶c̶e̶s̶.̶  
  -̶ *̶*̶I̶S̶C̶ D̶H̶C̶P̶*̶*̶:̶ I̶n̶s̶t̶a̶l̶l̶ o̶n̶ a̶ *̶*̶l̶i̶g̶h̶t̶w̶e̶i̶g̶h̶t̶ L̶i̶n̶u̶x̶ V̶M̶*̶*̶ (̶e̶.̶g̶.̶,̶ U̶b̶u̶n̶t̶u̶ 2̶2̶.̶0̶4̶ L̶T̶S̶,̶ B̶1̶s̶ t̶i̶e̶r̶)̶.̶  
    -̶ C̶a̶n̶ s̶h̶a̶r̶e̶ a̶ V̶M̶ w̶i̶t̶h̶ *̶n̶o̶n̶-̶c̶r̶i̶t̶i̶c̶a̶l̶ s̶e̶r̶v̶i̶c̶e̶s̶*̶ (̶e̶.̶g̶.̶,̶ m̶o̶n̶i̶t̶o̶r̶i̶n̶g̶ t̶o̶o̶l̶s̶)̶,̶ b̶u̶t̶ k̶e̶e̶p̶ i̶t̶ s̶e̶p̶a̶r̶a̶t̶e̶ f̶r̶o̶m̶ h̶i̶g̶h̶-̶l̶o̶a̶d̶ s̶e̶r̶v̶i̶c̶e̶s̶ l̶i̶k̶e̶ P̶o̶s̶t̶g̶r̶e̶s̶.̶  

---

Scratch that idea. Unfortunately Azure doesn't let you use PFSense while using their free plan. 

### WorkstationVM
- I setup Ubuntu 22 LTS for this.

---

### **3. Postgres Database**  
- Dedicated Linux VM Ubuntu  
  - Install Postgres manually (`sudo apt install postgresql`).  
  - Store data on attached Azure Managed Disks (SSD for performance).  


---

### **4. Storage Server**  
- **Recommendation**: **Separate VM from Postgres** (avoid resource contention).  
  - Use a Linux VM with:  
    - **Samba** (for Windows-compatible shares) or **NFS** (for Linux).  
    - Azure Blob Storage (optional) for cold/archive data.  

---

### **5. Domain & Identity Entra ID
- Entra ID
- Windows Domain Server

---

### **6. Email & Communication**  
- Microsoft365
- Phone/Teams/Text

---

