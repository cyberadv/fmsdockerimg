docker run                  \
    --detach                \
    --hostname   fms-docker \
    --name       fms-docker \
    --privileged            \
    --publish    80:80      \
    --publish    443:443    \
    --publish    2399:2399  \
    --publish    5003:5003  \
    --volume                \
        <installation_file_dir>:/install \
    --volume                \
        <shared_FM_dir>:"/opt/FileMaker/FileMaker Server/Data" \
    fmsdocker:prep

docker run  --detach  --hostname fms-docker --name fms-docker --privileged  --publish 80:80  --publish 443:443    --publish 2399:2399  --publish 5003:5003  --volume  C:\WRK_FMS\INSTALL:/install  --volume C:\WRK_FMS\DATA:"/opt/FileMaker/FileMaker Server/Data" fmsdocker:prep