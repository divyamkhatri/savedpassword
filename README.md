# savedpassword
import subprocess
import re
import pickle
import json
import time

command_output = subprocess.run(["netsh","wlan","show","profiles"],capture_output = True).stdout.decode()

profile_name = (re.findall("All User Profile     : (.*)\r",command_output))
wifi_dictionary = {}

for name in profile_name:

    profile_info = subprocess.run(["netsh","wlan","show","profile",name,"key=clear"],capture_output = True).stdout.decode()

    password = (re.findall("Key Content            : (.*)\r",profile_info))
    for passw in password:
       wifi_dictionary[name]=passw
wifi=(json.dumps(wifi_dictionary,indent=1))
pickle.dump(wifi,open("saved_pasword","wb"))
print(wifi)
print("This dictionary is stored as saved_password")
time.sleep(3)
