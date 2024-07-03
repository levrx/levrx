 ![1](https://cdn.discordapp.com/attachments/1252712386305982626/1258051828847874120/zzwrtim.png?ex=6686a391&is=66855211&hm=4732bd3170c441e0a3106fac1feb7898c00f5596fe5750488caeabf9fde37c3f&)

-   **Create a new partition on `/dev/sda`**:    
Use `fdisk` to create a new partition with the remaining space.

    `fdisk /dev/sda` 
    
    -   Type `n` to create a new partition.
    -   Follow the prompts to create a new primary partition.
    -   Change the partition type to Linux LVM by typing `t` and then selecting the code `8e`.
    -   Write the changes and exit by typing `w`.
 
-   **Create a physical volume** on the new partition (e.g., `/dev/sda6` if it's the next available partition):
    `pvcreate /dev/sda6` 
    
-   **Extend the volume group** to include the new physical volume:
     `vgextend vmtemplate--vg /dev/sda6` 
    
-   **Verify the volume group extension** and check the "Free PE / Size" field to confirm the additional space is available:
     `vgdisplay vmtemplate--vg` 
    
    
-   **Extend the root logical volume**:
    `lvextend -L +<size>G /dev/vmtemplate--vg/root` 
        
-   **Resize the filesystem** to use the additional space:
    `resize2fs /dev/vmtemplate--vg/root` 
    
-   **Verify the changes**:
    `df -h /`
