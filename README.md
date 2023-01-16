# autologin

    import schedule
    import time
    import requests
    from Cryptodome.Cipher import AES
    from Cryptodome.Random import get_random_bytes

    def login():
        # read the key and ciphertext from file
        with open("encrypted.bin", "rb") as f:
            nonce, tag, ciphertext = [ f.read(x) for x in (16, 16, -1) ]

        # create a cipher object using the key
        cipher = AES.new(key, AES.MODE_EAX, nonce)

        # decrypt the plaintext
        plaintext = cipher.decrypt_and_verify(ciphertext, tag)

        # the plaintext is now the original login credentials
        username,password = plaintext.decode().split(',')

        # Set login credentials
        payload = {'username': username, 'password': password}

        # Send post request to login page
        r = requests.post('https://website.com/login', data=payload)

        # Check if login is successful
        if r.status_code == 200:
            print('Login successful')
        else:
            print('Login failed')

    schedule.every().day.at("07:30").do(login)

    while True:
        schedule.run_pending()
        time.sleep(1)

This code is a Python script that uses the schedule, time, requests, and Cryptodome libraries to automate a login process. The script starts by importing the necessary libraries. Then, it defines a function called "login" that performs the following actions:

    It opens a file called "encrypted.bin" and reads the key, ciphertext, and tag from it.
    It creates a new AES cipher object using the key, nonce, and AES.MODE_EAX
    It uses the decrypt_and_verify method of the cipher object to decrypt the ciphertext and verify the tag.
    It splits the decrypted plaintext into two variables, username and password.
    It creates a payload variable to use for login with the username and password.
    It sends a post request to the login page of "https://website.com/login" with the payload.
    It checks if the status code of the response is 200, if yes then login is successful. Else it will print 'Login failed'

After defining the login function, the script uses the schedule library to schedule the login function to run every day at 7:30 AM. The script then enters an infinite loop that uses the run_pending() method of the schedule library to check if there are any scheduled tasks that need to be run, and the time.sleep(1) method to sleep for 1 second between each iteration of the loop.


## to create the encryoted.bin

    from Cryptodome.Cipher import AES
    from Cryptodome.Random import get_random_bytes

    # generate a random key
    key = get_random_bytes(16)

    # create a cipher object using the key
    cipher = AES.new(key, AES.MODE_EAX)

    # encrypt the plaintext
    ciphertext, tag = cipher.encrypt_and_digest(b'akunusername,passwoerdnyaapa')

    # write the key and ciphertext to file
    with open("encrypted.bin", "wb") as f:
        [ f.write(x) for x in (cipher.nonce, tag, ciphertext) ]


