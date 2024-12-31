import os
import subprocess
import time

def download_parrot_os():
    """Downloads the official Parrot OS ISO image."""
    url = "https://download.parrot.sh/parrot/iso/5.3/Parrot-security-5.3_amd64.iso"  # Update to the latest version if needed
    iso_path = "Parrot-security.iso"
    print(f"Downloading Parrot OS ISO from {url}...")
    subprocess.run(["wget", "-O", iso_path, url], check=True)
    print(f"Downloaded ISO saved at {iso_path}.")
    return iso_path

def create_bootable_usb(iso_path, usb_device):
    """Writes the ISO image to the USB device."""
    print(f"Creating bootable USB on {usb_device}...")
    subprocess.run(["sudo", "dd", f"if={iso_path}", f"of={usb_device}", "bs=4M", "status=progress"], check=True)
    subprocess.run(["sudo", "sync"], check=True)
    print("Bootable USB created successfully.")

def configure_automatic_installation(usb_device):
    """Sets up automated installation using a preseed file."""
    preseed_file = "/tmp/preseed.cfg"
    with open(preseed_file, "w") as file:
        file.write("""\
        d-i debian-installer/locale string en_US
        d-i keyboard-configuration/xkb-keymap select us
        d-i netcfg/choose_interface select auto
        d-i mirror/country string manual
        d-i mirror/http/hostname string deb.parrotsec.org
        d-i mirror/http/directory string /parrot
        d-i mirror/http/proxy string
        d-i passwd/root-password password root
        d-i passwd/root-password-again password root
        d-i clock-setup/utc boolean true
        d-i time/zone string Etc/UTC
        d-i partman-auto/disk string /dev/sda
        d-i partman-auto/method string regular
        d-i partman-auto/choose_recipe select atomic
        d-i partman/confirm boolean true
        d-i partman/confirm_nooverwrite boolean true
        d-i grub-installer/bootdev string /dev/sda
        d-i finish-install/reboot_in_progress note
        """)
    print("Preseed file created at /tmp/preseed.cfg.")

    # Copy preseed file to USB (assuming USB is mounted)
    usb_mount = f"/media/{os.getlogin()}/PARROT"
    if not os.path.exists(usb_mount):
        print(f"USB not mounted at {usb_mount}. Please ensure it is mounted.")
        return
    subprocess.run(["sudo", "cp", preseed_file, os.path.join(usb_mount, "preseed.cfg")], check=True)
    print("Preseed file copied to bootable USB.")

def main():
    try:
        # Step 1: Download Parrot OS ISO
        iso_path = download_parrot_os()

        # Step 2: Create bootable USB
        usb_device = input("Enter the USB device path (e.g., /dev/sdb): ")
        create_bootable_usb(iso_path, usb_device)

        # Step 3: Configure automated installation
        configure_automatic_installation(usb_device)

        print("Your bootable USB is ready to install Parrot OS as the sole OS!")
    except subprocess.CalledProcessError as e:
        print(f"An error occurred: {e}")
    except Exception as e:
        print(f"Unexpected error: {e}")

if __name__ == "__main__":
    main()
