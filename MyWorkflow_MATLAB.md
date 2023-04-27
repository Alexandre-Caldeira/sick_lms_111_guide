# My current workflow (wip)

```MATLAB
%% 0.1 Create TCP connection:
ip = '169.254.188.171'; % definido no sopaset
porta =2112; % porta de CoLa A, para comunicacao tcp/ip 
canal = tcpip (ip, ...
        'remoteport',porta, ... 
        'terminator',03, ... % 03 = fixed Hex value meaning user level “Authorized Client”
        'InputBufferSize',3000, ... % no idea why
        'OutputBufferSize',3000,... % no idea why
        'ReadAsyncMode','continuous', ... % Continuously query the instrument to determine if data is available to be read.
        'Timeout',5); % 5 sec
    
%% 0.2 Open TCP channel
fopen(canal)
canal

%% 1. Log in:                       (sMN SetAccessMode)
fwrite(canal,[02,... % telegram start header for CoLa A 
        'sMN SetAccessMode 03 F4724744',... % auth with password=F4724744 
        03]);  % telegram stop
    
fscanf(canal)

%% 2. Set frequency and resolution: (sMN mLMPsetscancfg)
% ATTENTION: Scan angle can not be changed here, only in the data output!This applies
% for LMS1xx and LMS5xx series.
    
% Scan frequency = 50 Hz
% Sectors = 1 sector (This value is always 1 for these devices)
% Angular resolution = 0,5°
% Start angle of sector = -45°(Fix values, angle not changeable)
% Stop angle of sector = 225° (Fix values, angle not changeable)
fwrite(canal,[02,... % telegram start header for CoLa A 
        'sMN mLMPsetscancfg +5000 +1 +5000 -450000 +2250000',... 
        03]);  % telegram stop
    
fscanf(canal)

%% 3. Configure scandata content:   (sWN LMDscandatacfg)


%% 4. Configure scandata output:    (sWN LMPoutputRange)


%% 5. Store parameters: sMN mEEwriteall



%% 6. Set timestamp/data angle:     (sMN LSPsetdatetime)
% The data format in the telegram is:
% +2009{SPC}+7{SPC}+22{SPC}+12{SPC}+0{SPC}+0{SPC}+0.
% The numbers represent year,month, day,hour,minute, second,microsecond).

% If plus is used up-front the data it is interpreted as an integer decimal number, without
% the plus it's the scanner reads the data as hex format.
% The answeris always in ASCII format.
% Attention: There is no real time clock inside the device. When the scanneris switched
% off and after a reboot,the time has to be set again.
% However, it is possible to analyze the Off-time in order to evade this issue.

fwrite(canal,[02,... % telegram start header for CoLa A 
       ['sMN LSPsetdatetime ',char(datetime('now','Format','+y +M +d +HH +mm +ss')),' 0'],... 
        03]);  % telegram stop
    
% Answer: sRA STlms, table on manual page 90
fscanf(canal)


%% 7. Log out: sMN Run 


%% CHECK: Read time stamp and status of the measurement function
fwrite(canal,[02,... % telegram start header for CoLa A 
            'sRN STlms',... 
        03]);  % telegram stop
    
% Answer: sRA STlms, table on manual page 90
fscanf(canal)

%Ex:  'sRA STlms 7 0 8 00:11:50 A 01.01.1970 0 0 0'

%% 8. Request scan:
%     - poll one telegram:         (sRN LMDscandata)
while true
   
fwrite(canal,[02,... % telegram start header for CoLa A 
            'sRN LMDscandata',... 
        03]);  % telegram stop
    
% Answer: sRA STlms, table on manual page 90
single_scan = fscanf(canal);

data = strsplit(single_scan(2:end-1),'1388 21D');
header = data(1); scan = cell2mat(data(2));
scan = strsplit(scan(2:end));
measurements = zeros(1,length(scan));
for i= 1:length(scan)
    val = hex2dec(scan(i));
    if size(val,1)>0
        measurements(i) = val;
    else
        measurements(i) = -1;
    end
end
% if strfind(scan
plot(measurements)
drawnow
pause(1)
end

%% 8. Request scan (permanently):
%     - send data permanently:     (sEN LMDscandata)

```