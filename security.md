###The sudo command
Granting privileges using the ``sudo`` command is less dangerous than ``su`` and it should be preferred. By default, ``sudo`` must be enabled on a per-user basis. However, some distributions (such as Ubuntu) enable it by default for at least one main user, or give this as an installation option. To execute just one command with root privilege type ``sudo <command>``. When the command is complete you will return to being a normal unprivileged user. The ``sudo`` configuration files are stored in the ``/etc/sudoers`` file and in the ``/etc/sudoers.d/`` directory. By default, that directory is empty.



