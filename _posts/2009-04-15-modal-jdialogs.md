---
layout: post
title: Modal JDialogs
---

Recently our customers experienced problems running our software after updating to JRE6U12. In detail on Windows XP the desktop application would hang directly after connecting to the database. Downgrading the RE to U11 fixed the problem so we first thought it was a bug in the JRE.

Since google investigation did not bring any results except this we were convinced it should be our fault. After several hours of checking swing calls outside of the evnt dispatch thread, checking for undisposed dialogs and hunting down modal dialogs we found the following two statements.

    dialog.setVisible(true);
    dialog.setModal(true);

Do you see the problem? Neither did we at first. And since the code had worked before we did not think the order of these two statements would be the problem.

In fact it does not make much sense to set a dialog to modal **after** making it visible, but still these two lines were the cause of the main window losing focus, not reacting to input events any more, and for the user being unable to reactivate the main window. To the user this looks like a freeze of the application.

So now, after several hours of debugging, we know that we must call `setVisible(true)` **after** `setModal(true)`!
