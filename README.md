🔐 Encrypt an External Drive on Linux (LUKS)

    ⚠️ All existing data will be erased. Back it up before proceeding!

✅ Prerequisites

    An external drive (e.g. /dev/sdX)

    Root/sudo access

    Package cryptsetup (usually preinstalled)

🛠 Step-by-Step Instructions
1. Identify your external drive

Run:

```bash
lsblk
```

Look for your external device (e.g. /dev/sdb). Make sure it's the correct device — don't guess!
2. (Optional but recommended) Wipe the drive

This helps protect against data recovery from old files:

```bash
sudo dd if=/dev/zero of=/dev/sdX bs=1M status=progress
```

Replace /dev/sdX with your drive (e.g. /dev/sdb). This could take a while.
3. Encrypt the drive with LUKS

```bash
sudo cryptsetup luksFormat /dev/sdX
```

    Type YES in uppercase when prompted.

    Set a strong passphrase.

📝 If you're using a partition, specify it (e.g. /dev/sdX1 instead of /dev/sdX).
4. Open the encrypted device

```bash
sudo cryptsetup open /dev/sdX my_external
```

This creates a mapper device: /dev/mapper/my_external
5. Format the encrypted volume

```bash
sudo mkfs.ext4 /dev/mapper/my_external
```

You can also use xfs, btrfs, etc.
6. Mount it

```bash
sudo mkdir /mnt/encrypted
sudo mount /dev/mapper/my_external /mnt/encrypted
```

Now you can use it like any mounted drive.
7. Unmount and close when done

```bash
sudo umount /mnt/encrypted
sudo cryptsetup close my_external
```

🔁 Next Time You Plug It In

When you reconnect the drive:

    Run:

```bash
sudo cryptsetup open /dev/sdX my_external
```

Mount it:
  ```bash
  sudo mount /dev/mapper/my_external /mnt/encrypted
  ```

🔒 Optional Extras

    Add multiple keys:

```bash
sudo cryptsetup luksAddKey /dev/sdX
```

Use a keyfile instead of password (for headless servers)

Automount on certain systems with udisks / udisksctl / gnome-disks
