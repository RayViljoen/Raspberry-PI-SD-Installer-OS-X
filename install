#!/bin/bash

# Raspberry PI Install Script
# Author: Ray Viljoen

# Quit on error
set -e

_piline="------------------------------------------------------------------------------------"

# Display colorized information output
function _info() {
    COLOR='\033[00;32m' # green
    RESET='\033[00;00m' # white
    echo -e "${COLOR}${@}${RESET}"
}
 
# Display colorized warning output
function _warn() {
    COLOR='\033[00;31m' # red
    RESET='\033[00;00m' # white
    echo -e "${COLOR}${@}${RESET}"
}

_info '
__________.___  .___                 __         .__  .__                
\______   \   | |   | ____   _______/  |______  |  | |  |   ___________ 
 |     ___/   | |   |/    \ /  ___/\   __\__  \ |  | |  | _/ __ \_  __ \
 |    |   |   | |   |   |  \\___ \  |  |  / __ \|  |_|  |_\  ___/|  | \/
 |____|   |___| |___|___|  /____  > |__| (____  /____/____/\___  >__|   
                         \/     \/            \/               \/       
'


# Set $1 as image path
DISTRO="$1"

# Check image path is set else exit
if [ -z "$DISTRO" ]; then
    _warn $_piline
    _warn "ERR: No image path supplied"
    _warn $_piline
    exit 0
fi



# Selected disk
_udisk=""

function promptDisk() {

    # ================================================
    # Check connected disks and save paths to array
    # ================================================

    # Counter
    i=0

    echo $_piline

    # Loop over df -lh
    while read line; do

        # Print with local mount only and add counter to select disk
        if [ "$i" -gt 0 ]; then
            echo "$i) $line"
            # Asign first work (path) to array with corresponding counter
            _mount[i]=$( echo $line | awk '{print $1;}')
        else
            _info "   $line"
            echo $_piline
        fi

        # Increment counter
        ((i++))

    done <<< "$(df -h)"
    echo -e "$_piline\n"

    _opts=''
    for i in "${!_mount[@]}"; do
        [ -z "$_opts" ] && _opts="${_opts}${i}" || _opts="${_opts}, ${i}"
    done

    # Ask user to select mounted disk
    echo "Select the disk to use by enetering the disk number."
    _warn "*** MAKE SURE YOU SELECT THE CORRECT DISK ***"
    _warn "*** Refer to the Readme if uncertain ***"
    echo -n -e "\nUse disk [ $_opts ] #"
    read ans

    # Set selected disk
    _udisk=${_mount[$ans]}

    # Test if valid disk selected
    if [ -z "$_udisk" ]; then
        _warn "\n ======= Invalid selection ======= \n"
        promptDisk
    fi
}

# Run prompt
promptDisk

# ===========================================================
# Past this point a valid disk has been selected, so proceed.
# ===========================================================

# Format disk name to raw format
_rawdisk=$( echo $_udisk | awk 'sub("..$", "")' | sed 's/disk/rdisk/')

# Unmount Disk
echo "Unmounting Disk"
diskutil unmount $_udisk

echo "Writing image"
echo "Ctrl+T to see progress.."
sudo dd bs=1m if=${DISTRO} of=${_rawdisk}

# Eject disk
echo "Ejecting Disk"
diskutil eject ${_rawdisk}

_info "All Done!"