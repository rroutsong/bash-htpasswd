#!/bin/bash
usage() {
    echo -e "\nhtpasswd [option] [username] [password]    (1st form)"
    echo -e "htpasswd [username] [password]               (2nd form)"
    echo -e "htpasswd -f [filename] [username] [password] (3rd form)\n"
    echo -e "Options:"
    echo -e "\t-m, generate md5 passwords (default)"
    echo -e "\t-s, generate SHA1 passwords"
    echo -e "\t-c, generate CRYPT passwords"
    echo -e "\t-f, append output to existing file\n"
    echo -e "If used without options, this script will return [username]:[md5 password hash]\n"
}

# check whether user had supplied -h or --help . If yes display usage
if [[ ( $# == "--help") ||  $# == "-h" ]]
then
    usage
fi

if [ $# -le 1 ]
then
    usage
fi

# if two arguments supplied, display username:md5 hash
if [  "$#" == 2 ]
then
    eval "printf $1:\$(openssl passwd -apr1 $2)\\\n"
fi

if [ $# -ge 3 ]
then
    while getopts ":fmsc" opt; do
        case $opt in
            f)
                echo "printf $3:\$(openssl passwd -apr1 $4)\\\n >> $2"
                #eval "printf $3:\$(openssl passwd -apr1 $4)\\\n >> $2"
                ;;
            fm)
                eval "printf $3:\$(openssl passwd -apr1 $4)\\\n >> $2"
                ;;
            m)
                echo "printf $3:\$(openssl passwd -apr1 $4)\\\n >> $2"
                ;;
            fs)
                eval "printf $3:\$(echo -n $4 | sha1sum | awk '{print $1}')\\\n >> $2"
                ;;
            s)
                echo "printf user:\$(echo -n $4| sha1sum | awk '{print $1}')\\\n"
                #eval "printf user:\$(echo -n $4| sha1sum | awk '{print $1}')\\n"
                ;;
            fc)
                eval "printf $3:\$(openssl passwd -crypt $4)\\\n >> $2"
                ;;
            c)
                echo "printf $3:\$(openssl passwd -crypt $4)\\\n >> $2"
                ;;
            \?)
                echo "invalid option -$OPTARG" >&2
                ;;
        esac
    done
fi
