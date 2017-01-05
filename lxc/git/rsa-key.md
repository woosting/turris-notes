SSH looks for the user's `~/.ssh/config` file. It will use the default (~/.ssh/id_rsa) if not specified otherwise. So set it up as followed:
 
    Host github.com
        Hostname github.com
        IdentityFile ~/.ssh/id_rsa.github
        IdentitiesOnly yes # see NOTES below

### NOTES
- The `IdentitiesOnly yes` is required to [prevent the SSH default behavior][2] of sending the identity file matching the default filename for each protocol. If you have a file named `~/.ssh/id_rsa` that will get tried BEFORE your `~/.ssh/id_rsa.github` without this option.

### References

- [Best way to use multiple SSH private keys on one client][1]
- [How could I stop ssh offering a wrong key][2]

[1]: http://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client
[2]: http://serverfault.com/questions/450796/how-could-i-stop-ssh-offering-a-wrong-key/450807#450807 "foo"
