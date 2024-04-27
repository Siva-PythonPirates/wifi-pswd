# wifi-pswd
import subprocess
import re

def get_wifi_profiles():
    try:
        result = subprocess.run(["netsh", "wlan", "show", "profiles"], capture_output=True, text=True, check=True)
        profile_names = re.findall(r"Profile\s*:\s*(.*)", result.stdout)
        return profile_names
  
    except subprocess.CalledProcessError as e:
        print(f"Error occurred while listing Wi-Fi profiles: {e}")
        return []

def get_wifi_password(profile_name):
    try:
        # Execute netsh command to show key content for the specified profile
        result = subprocess.run(["netsh", "wlan", "show", "profile", profile_name, "key=clear"], capture_output=True, text=True, check=True)
        
        # Extract key content (password) using regular expression
        key_content_match = re.search(r"Key Content\s*:\s*(.*)", result.stdout)
        if key_content_match:
            key_content = key_content_match.group(1).strip()
            return key_content
        else:
            return "Password not found"
    
    except subprocess.CalledProcessError as e:
        print(f"Error occurred while retrieving password for '{profile_name}': {e}")
        return "Error"

# Get list of Wi-Fi profiles
wifi_profiles = get_wifi_profiles()

# Iterate over each Wi-Fi profile and retrieve password
for profile_name in wifi_profiles:
    key_content = get_wifi_password(profile_name)
    print(f"Wi-Fi Profile: {profile_name}")
    print(f"Key Content: {key_content}")
    print("-" * 30)
    
