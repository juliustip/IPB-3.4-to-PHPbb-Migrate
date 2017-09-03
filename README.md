# IPB 3.4 to PHPbb Migrate

https://www.phpbb.com/community/viewtopic.php?f=561&t=2400741&p=14816231#p14816231

<b>NOTE: THIS IS NOT TO BE LEFT ON SERVER AFTER USE!</b>

INSTRUCTIONS FROM AUTHOR:

Converting from IPB 3.4 to phpBB 3.2 proved quite a challenge, mainly because of the sheer size of the board, along with some dedicated stuff that was implemented. I took these steps:

  1.  Backup current state IPB (files + db)
  2.  Install phpBB 3.0.12custom {phpbb_3012_custom.zip}
  3.  Run convertor for IPB to phpBB 3.0.12 {ipb34_convertor_0.0.1.zip} (https://www.phpbb.com/community/viewtopic.php?p=13539291#p13539291)
        In ./install/install_convert.php you find the vars $batch_size & $num_rows. Play with these a bit. I noticed that I can ramp them up to about 20000 resp. 600 for the most part, but I need to lower them to the default 6000 resp. 60 when the convertor approaches post_skip_rows ± 700000.
        This depends on your board and server, so make sure you run some tests to prevent timeouts on your board.
   4. Passwords will be reset since I don't want the unsafe triple MD5() that IPB uses to keep working. Therefore, when the conversion to phpBB 3.0.12 has completed, you should immediately request a new password for your own admin account. In the 3.0.12.custom files I've changed the protocol to show your new password on screen and be activated immediately.
    I shouldn't have to write this, but anyway: make sure you are the only one who can do this. Make your installation unreachable for the general public.
   5. Backup current state
   6. Fast update to phpBB 3.1.10.custom:
        Remove all files from the phpBB dir except config.php and the folders files, images, store.
        Copy the 3.1.10 files to the phpBB dir except above items
        Goto ./install/database_update.php and let it complete
        Backup current state
   7. Fast update to phpBB 3.2.0.custom:
        Remove all files from the phpBB dir except config.php and the folders files, images, store.
        Copy the 3.2.x files to the phpBB dir except above items
        I've found that the CLI update is easiest: php ./bin/phpbbcli.php db:migrate --safe-mode --> Seems to hang some time half way, but let it go; this is the alteration to the topics table, which is huge.
        Remove the install dir
   8. Run the text reparser (optional): php bin/phpbbcli.php reparser:reparse --ansi (this can take quite some time on large boards, but is also the reason to do it manually instead of the usual cron method)
   9. Install, run and remove the extension ger/syncforums (incorporated in the download). This is needed I kinda hacked the convertor to speed up stuff. It recalculates post/topic counters etc. It's not the most proper extension, so make sure to remove it after use. I only intended this for personal use in a protected environment.
   10. Set permissions, groups, etc. to your liking
   11. Configure spambot prevention
   12. Check all other settings

The whole process took about 4 hours from start to finish for a board with ± 1 million posts and 50.000 users.
