## Ansible role dependencies with tags

Here is a small playbook that should help with debugging of
[Ansible](https://github.com/ansible/ansible/) roles with dependencies used
with tags.

### Full playbook run

First, run the playbook without any tags to see what happens:

`ansible-playbook -i hosts play.yml`

Everything should work OK by this point. Example output:

```
PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Testing, testing..."
}

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What will be said goes here."
}

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What do we want?"
}

TASK: [person | Person says] ************************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*phphhph*"
}

TASK: [crowd | Crowd screams] ************************************************* 
ok: [localhost] => {
    "item": "", 
    "msg": "Ansible Roles!"
}

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Disperse this illegal demonstration at once!"
}

TASK: [police | Police officer says] ****************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*kkfdkg ffhd gkkff hkhhhkhk*"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=8    changed=0    unreachable=0    failed=0   
```

### Playbook with tags on ansible 1.5.0

Now, let's try running different roles using tags, on **Ansible 1.5.0**:

#### Just the speaker

```
ansible-playbook -i hosts play.yml --tags speaker

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Testing, testing..."
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

#### Just the microphone

```
ansible-playbook -i hosts play.yml --tags microphone

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What will be said goes here."
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

#### Just a person that speaks to the crowd

```
ansible-playbook -i hosts play.yml --tags person

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What do we want?"
}

TASK: [person | Person says] ************************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*phphhph*"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=3    changed=0    unreachable=0    failed=0 
```

#### Just the crowd answering the person

```
ansible-playbook -i hosts play.yml --tags crowd

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [crowd | Crowd screams] ************************************************* 
ok: [localhost] => {
    "item": "", 
    "msg": "Ansible Roles!"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0
```

#### And the police officer that tries to keep the peace

```
ansible-playbook -i hosts play.yml --tags police

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Disperse this illegal demonstration at once!"
}

TASK: [police | Police officer says] ****************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*kkfdkg ffhd gkkff hkhhhkhk*"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=3    changed=0    unreachable=0    failed=0   
```

### Playbook with tags on ansible 1.6 (devel)

Let's see how things change with current devel branch of Ansible:

#### Just the speaker

```
ansible-playbook -i hosts play.yml --tags speaker

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Testing, testing..."
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

#### So far, so good. The microphone now

```
ansible-playbook -i hosts play.yml --tags microphone

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What will be said goes here."
}

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "What do we want?"
}

TASK: [speaker | Audio message] *********************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "Disperse this illegal demonstration at once!"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=4    changed=0    unreachable=0    failed=0   
```

#### Something's not right, microphone speaks by itself. Now, the person at the stand

```
ansible-playbook -i hosts --tags person

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [person | Person says] ************************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*phphhph*"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

#### He says something, but nobody hears him. The crowd is next

```
ansible-playbook -i hosts play.yml --tags crowd

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [crowd | Crowd screams] ************************************************* 
ok: [localhost] => {
    "item": "", 
    "msg": "Ansible Roles!"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

#### They are as happy as ever. And now, the police officer

```
ansible-playbook -i hosts play.yml --tags police

PLAY [localhost] ************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [police | Police officer says] ****************************************** 
ok: [localhost] => {
    "item": "", 
    "msg": "*kkfdkg ffhd gkkff hkhhhkhk*"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=2    changed=0    unreachable=0    failed=0   
```

Police won't be dispersing this crowd today.

### The end

That's all for now, I'm going back to `git bisect` this thing...

