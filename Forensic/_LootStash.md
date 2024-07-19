Spawn the docker and below spawn button there is download file option download the resources.

You will get pcaf file.

Open with Wireshark to analyze or use NetworkMiner to analyze 

![[Pasted image 20240718022911.png]]

On message there is SecureFile transfered through mail.

Get all secure file (total 15).
password of that zip file will be on message

extract all and you will get phreaks_plan.pdf.part1 - 15

then combine all parts to get a pdf

Used Code:
``` python
import zipfile

import os

  

# List of zip files and their corresponding passwords

zip_files = []

#Passwords may be different according to pcap files
passwords = [

    'r5Q6YQEcGWEF','r5Q6YQEcGWEF','jISlbC8145Ox','AdtJYhF4sFgv','j2SRRDraIvUZ',

    'xh161WSXX7tB','yH5vqnkm7Ixa','tJPUTUfceO1P','2qKlZHZlBPQz','mbkUvLZ1koxu',

    'ZN4yKAYrtf8x','0eA143t4432M','oea41WCJrWwN','gdOvbPtB0xCK'

]

  

# Create directories based on passwords

for password in passwords:

    folder_name = password

    os.makedirs(folder_name, exist_ok=True)

  

# Generate zip file information with corresponding passwords

for i in range(1, 15):

    filename = f"SecureFile[{i}].zip"

    password = passwords[i-1]  # Using passwords list, adjusted for zero-based index

    zip_files.append({"filename": filename, "password": password})

  

def extract_zip(file_info):

    filename = file_info["filename"]

    password = file_info["password"]

  

    try:

        with zipfile.ZipFile(filename) as zf:

            zf.extractall(pwd=password.encode())

            print(f"Successfully extracted {filename}")

    except zipfile.BadZipFile:

        print(f"{filename} is not a valid zip file")

    except zipfile.LargeZipFile:

        print(f"{filename} is too large to handle")

    except RuntimeError as e:

        print(f"Error extracting {filename}: {e}")

    except Exception as e:

        print(f"Unexpected error extracting {filename}: {e}")

  

# Extract each zip file with its corresponding password

for file_info in zip_files:

    extract_zip(file_info)
```


``` python
for i in range(1, 16): #one for each pdf.part
    print(f"phreaks_plan.pdf.part{i}")
```

run this using
``` cmd
python ./file.py > files.txt
```

then you will get files.txt containing the name of all file part.

Then,
```bash
#!/bin/bash

if [ ! -f files.txt ]; then
    echo "Error: files.txt not found."
    exit 1
fi

if [ ! -f phreaks_plan.pdf ]; then
    touch phreaks_plan.pdf
fi

while IFS= read -r filename; do
    if [ -f "$filename" ]; then
        cat "$filename" >> phreaks_plan.pdf
        echo "Content of $filename appended to phreaks_plan.pdf"
    else
        echo "Error: $filename not found."
    fi
done < files.txt

echo "All files processed."
```

save as assemble.sh and give executable permission to it.
```cmd
chmod +x assemble.sh
```

then run it.
```cmd
./assemble.sh
```

you will get a pdf name phreaks_plan.pdf containing key.



Used Tools: Wireshark, NetworkMiner.
Used Languages: Python
Used OS: Kali Linux

Key:
HTB{Th3Phr3aksReadyT0Att4ck}