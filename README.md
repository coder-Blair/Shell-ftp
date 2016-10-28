#Shell-ftp
#!/bin/bash
 
i=1
while true
do
 nc -z 172.16.9.26 21 &>/dev/null
 if  (( $? != 0 ))
 then
  echo "warning!!!"   
  date_warn=$(date +%s)
  [[ -s ftp_warning_log.txt ]] || echo "${date_warn} the ftp is down!" >ftp_warning_log.txt
 else
  if [ -s ftp_warning_log.txt ]
  then
   date_good=$(date +%s)
   echo $date_good
   echo "ok!"
   time_warn=$(cat ftp_warning_log.txt|awk -F " "  '{print $1}')
   echo ${time_warn}
  ((date_cha=date_good-time_warn))
   echo -e "$i\t ftp down start time:$time_warn;down stop time:$date_good;down total time:${date_cha}"  >>time_total.txt
   (( i++ ))
   echo $i >i.txt
  else
   echo "it's ok"
  fi
   >ftp_warning_log.txt
 fi
 
 sleep 1
 i_num=`cat i.txt`
 if  ((i_num==1))||((i_num==i))
 then
  echo "the ftp is always down ,begain time is $time_warn" >>time_total.txt
 fi
done
