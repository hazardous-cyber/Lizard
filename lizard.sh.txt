#!/bin/bash

if [[ $UID !=0 ]];
    then
    echo "Script must be run with sudo"
    exit 1
fi

declare jsonarray
declare gzarray
for file in /home/repo/*; do
    if [[ $file == *.gz ]]
        then
        gzarray+=($file)
    elif [[ $file == *.json ]]
        then
        jsonarray+=($file)
    fi
done

echo "JSON FILES: ${jsonarray[@]}"
echo "GZ FILES: ${gzarray[@]}"

if [ ${#gzarray{@]} -eq 0 ];
    then
    echo "There are zero GZ files to send"
else
    if [ "$1" != "" ];
    then
        scp ${gzarray[@]} $1@ipaddress:/home/repo2
    else
        echo "Enter the SSH user as a command line argument"
    fi
fi

if [ ${#jsonarray[@]} -eq 0 ];
    then
    echo "There are zero JSON files to send"
else
    if [ "$1" != "" ];
    then
        scp ${jsonarray[@]} $1@ipaddress:/home/repo2
    else
        echo "Enter the SSH user as a command line argument"
    fi
fi
