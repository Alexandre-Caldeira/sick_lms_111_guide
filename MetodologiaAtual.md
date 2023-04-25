# Metodologia em Etapas
Codigo de comunicacao com o sensor, da forma como é feita atualmente:

```MATLAB
%% stablish connection

ip = '169.254.188.171'; % definido no sopaset
porta =2112; % porta de CoLa A, para comunicacao tcp/ip 
canal = tcpip (ip, ...
        'remoteport',porta, ... 
        'terminator',03, ... % 03 = fixed Hex value meaning user level “Authorized Client”
        'InputBufferSize',3000, ... % no idea why
        'OutputBufferSize',3000,... % no idea why
        'ReadAsyncMode','continuous', ... % Continuously query the instrument to determine if data is available to be read.
        'Timeout',5); % 5 sec
canal
fopen(canal)
canal

%% auth
fwrite(canal,[02,... % telegram start header for CoLa A 
        'sMN SetAccessMode 03 F4724744',... % auth with password=F4724744 
        03]);  % telegram stop
canal

%% start scanning
fscanf(canal)
fwrite(canal,[2 'sMN LMCstartmeas 1' 3])  % read distances (??)
fwrite(canal,[2 'sEN LMDscandata 1' 3]) % rotate mirror (??)
fscanf(canal)


%% get data



%% close

fclose(canal)
```