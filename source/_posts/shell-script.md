---
title: Writing shell scripts
date: 2018-09-18 23:57:02
categories: 吴带当风
tags: Linux
---

## Introduction

Shell scripting makes life more comfortable. The command sequences of Linux can be comblined in one shell script and be executed automatively without wasting any time. For example, if you would like to rename 9,999,999 files and put them into different directories during one week, clicking the mouse will be devastating. By shell scripting, all you need is to run one command in a loop. After that, you can have a vacation and enjoy coeffee in the next seven days.

Suppose we have known at aleast a little bit about Linux. What is a Linux shell? Shell is an command language interpreter that executes commands. The most commonly used shells are SH (Bourne SHell) and Bash (Bourne again shell), released in 1989. Bash is not a perfect scripting language, but it is very useful. Life is short, get rid of the mouse.
<!-- more -->
## Shell Scripting Basics

Here are some examples and templates of the shell scripting to take care of the files and directories. 

    # reading the content of file1 and the output is written to file2 (instead of being displayed on screen)
    cat file1 > file2
    
    # reading the content of file1 and the output is added to the end of file2
    cat file1 >> file2
    
    # concatenation and alphabetize (from a to z or from 1 to infinite) the lines of text after concatenation
    cat file1 file2 file3 | sort > file4

    # deleting a directory excluding files (no "-r")
    rmdir directory

    # unpack
    tar -zxvf file.tar.gz

    # pack directories or files to a file archiver
    tar -zcvf test.tar.gz directory file1 file2...

    # search a file name in a directory
    find ~/ -name "*.sh"

    # search for a certain text within files and show the line number
    grep "money*" filename -n

    # search for a special file without a character, e.g. we have a@0.sim, a@1.sim, a.sim
    ls ./*.sim | grep -v @

    # displaying all currently running processes (-f is equal to -A) / show details / certain process / by certain user
    ps -A
    ps -aux
    ps -ef | grep "firefox"
    ps -u root

    # conditional statement used to examine a status of a file
    if [ ! -f "$myFile" ]; then
    touch "$myFile"
    fi
    # -a file 	Returns true if file exists (or -e)
    # -d file 	Returns true if file exists and is a directory
    # -s file 	Returns true if file exists and has a greater size that zero
    # -r file 	Returns true if file exists and is readable
    # -w file 	Returns true if file exists and is writable
    # -x file 	Returns true if file exists and is executable
    # -N file 	Returns true if the file exists and has been modified since it was last read

    # change file name using sed
    for file in `ls *.eps`
    do 
        mv $file `echo $file | sed 's/old/new/g'`
    done




## File Handling Primer

### Example 1: delete, replace, grep...

    #!/bin/bash
    
    # This is a script to read the pressure information from a large amount of log files and write the results.
    
    #----------------------------------------#

    echo '--> Start reading the minimum pressure and its location: (x,y)'

    # get the geometry name:
    var=./*.IGS

    # add .IGS means to ignore the file type
    region_name=$(basename $var .IGS)

    # '#*-' means delete the left side from the first left '-'
    # '##*-' means delete the left side from the last left '-'
    case_name_temp=${region_name#*-}

    # // means replacing all, / means replacing the first element, the second / is a separator
    case_name=${case_name_temp//-/_}"_steady@15000.sim"

    log_name="minimum_pressure_log_"${region_name}".log"

    echo '--> The geometry in this case is named as:' $region_name
    echo '--> The case name using the latest .sim file is:' $case_name

    # use " instead of ' to get the variable in shell
    sed -i "s/geometry/$region_name/g" Test_Minimum_Pressure_Location.java

    # note that ` is not '
    # search for the first line containing "Total:" in the log file
    minimum_cp_temp=`grep "Total:" $log_name | grep -n "" | grep "^1"` 
    # delete the left side from the last left space character
    minimum_cp_origin=${minimum_cp_temp##* }
    # transfer the data from .12345678 to 0.12345678
    minimum_cp=`echo $minimum_cp_origin | awk '{printf("%.8f\n",$0)}'`  

    echo 'minimum_cp is:' $minimum_cp

    # search for the second line containing "Total:" in the log file
    x_minimum_cp_temp=`grep "Total:" $log_name | grep -n "" | grep "^2"` 
    x_minimum_cp_origin=${x_minimum_cp_temp##* }
    x_minimum_cp=`echo $x_minimum_cp_origin | awk '{printf("%.9f\n",$0)}'`

    echo 'x_mp is:' $x_minimum_cp

    # search for the third line containing "Total:" in the log file
    y_minimum_cp_temp=`grep "Total:" $log_name | grep -n "" | grep "^3"` 
    y_minimum_cp_origin=${y_minimum_cp_temp##* }
    y_minimum_cp=`echo $y_minimum_cp_origin | awk '{printf("%.9f\n",$0)}'`

    echo 'y_mp is:' $y_minimum_cp

    # note: > means replace the old file, >> will add the old file
    echo 'minimum_cp x_mp y_mp' >> result_minimum_pressure_location.dat
    echo "$minimum_cp $x_minimum_cp $y_minimum_cp" >> result_minimum_pressure_location.dat

### Example 2: array, loop...

    #!/bin/bash

    # This is a job submitting script on the cluster of Skipper
    # The array can be used only in bash, not in sh
    
    #----------------------------------------#
    
    # get the subdirectory and ignoring the files
    sub_directory=`ls -l | egrep '^d' | awk '{print $(NF)}'`

    echo 'The directories of the angle of attack are:' $sub_directory

    # get the avaliable node_info of skipper
    node_number=(26 27 28 22 23)
    core_number=(52 52 52 28 28)

    # index is from 0 to N-1 of all cases
    index=0

    for allangle in $sub_directory 

      do 
      # arrange the node and cores of skipper 

      # $[] means arithmetic computation: index%5 loop
      # ${node_number[@]} returns the element of array
      # ${#node_number[@]} returns the element number of array

      i=$[index%${#node_number[@]}]

      # example: cd ./P4.00/
      cd $allangle

      node_info="nodes=compute-0-"${node_number[$i]}":ppn="${core_number[$i]}

      # the present directory
      directory_name=`pwd`
 
      # get the geometry name, case name, job name and log name 
      csv_name=./*.csv
  
      geometry_name=$(basename $csv_name .csv)
  
      case_name="steady_"${geometry_name}".sim"
  
      submit_job_name="job_"${geometry_name}".sh"
  
      log_name="logfile_"${geometry_name}".log"
  
      # configure the job script

      cp ../submit_job_template_skipper.sh ./$submit_job_name

      # the directory name has the symbol "/", so we use "#" as the separator in sed command
      sed -i "s#arg_case_directory#$directory_name#g" ./$submit_job_name 
      sed -i "s/arg_node_info/$node_info/g" ./$submit_job_name 
      sed -i "s/arg_case_name/$case_name/g" ./$submit_job_name 
      sed -i "s/arg_log_name/$log_name/g" ./$submit_job_name 
  
      # configure the StarCCM+ script
      cp ../autorun_naca66mod.java ./
  
      sed -i "s/arg_geometry_name/$geometry_name/g" ./autorun_naca66mod.java
      sed -i "s#arg_case_directory#$directory_name#g" ./autorun_naca66mod.java
  
      echo "--> Present case is: $case_name"
      echo "--> Present directory is: $directory_name"
      echo "--> Present computation is on the node: $node_info"
  
      # qsub $submit_job_name
      echo "--> Present case is submitted!"
  
      cd ../

      # update the index of the node array
      # many ways to calculate i++: let i+=1; ((i++)); i=`expr $i + 1`; i=$(( $i + 1 )); i=$[i+1]
      index=$[index+1]

    done



## Run a Shell Script 

Set the execute permission on your script: 
    
    chmod u+x yourname.sh
    ./yourname.sh

Enjoy it.



## Advanced Shell Script

### Remove all files/directories except for one file

- rm files except 'file.txt'

    find . ! -name 'file.txt' -type f -exec rm -f {} +

- rm files except 'file.txt'

    find . ! -name 'file.txt' -type d -exec rm -rf {} +

In zsh, you must enable extendedglob

    setopt extendedglob && rm -- ^file.txt





