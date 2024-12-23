```bash
#!/bin/bash

user_list=("nikola" "karlos" "omar" "emil" "amanda" "emilia")
user_group="devs"

if [ "$(id -u)" -ne 0 ]
    then echo "Run the scripts as sudo!"
    exit
fi

spinner () {
    local pid=$!
    local delay=0.1
    local message=$2
    local spinstr='|/-\\'  # Spinner symbols
    while kill -0 $pid 2>/dev/null; do
        local temp=${spinstr#?}
        printf "\r$message [%c]  " "$spinstr"  # Update the spinner on the same line
        spinstr=$temp${spinstr%"$temp"}
        sleep $delay
    done
    printf "\r$message [✓]  \n"  # Add a newline at the end
}

create_users () {
    read -r -p "Enter the default password for the user accounts: " default_password
    for user in "${user_list[@]}"; do
        if ! id "$user" > /dev/null 2>&1
        then
            (adduser --gecos "" --disabled-password "$user" > /dev/null 2>&1
            echo "$user:$default_password" | chpasswd > /dev/null 2>&1
            passwd --expire "$user" > /dev/null 2>&1) & spinner $! "Adding user/s"
        else
            echo "$user already exists unchanged password"
        fi
    done
}

create_group_and_add_users () {
    for user in "${user_list[@]}"; do
        if id "$user" > /dev/null 2>&1
        then
            if [ ! "$(getent group $user_group)" ] 
            then
                groupadd $user_group > /dev/null 2>&1
            else
                if ! id -nG $user | grep $user_group > /dev/null 2>&1
                then
                    (usermod -aG "$user_group" "$user" > /dev/null 2>&1) & spinner $! "$user have been added to the group $user_group"
                fi
            fi
        fi
    done
}

create_shared_folder () {
    if [ ! -d "/opt/$user_group" ]
    then
        #echo "Creating the shared folder at: /opt/$user_group"
        (mkdir /opt/$user_group
        chgrp -R devs /opt/$user_group
        chmod -R g+srwx /opt/$user_group) & spinner $! "Creating the shared folder at: /opt/$user_group"
    fi
}

update_packages_and_install_ssh_server () {
    (apt update > /dev/null 2>&1; apt upgrade -y > /dev/null 2>&1; apt install openssh-server -y > /dev/null 2>&1) & spinner $! "Fetching latest packages and installing openssh-server"
}

create_users
create_group_and_add_users
create_shared_folder
update_packages_and_install_ssh_server
```