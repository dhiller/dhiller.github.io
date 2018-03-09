---
layout: post
title: Warum ich TDD liebe
summary: Warum ich TDD liebe.
tags: [tdd,xp,german]
---

Ich liebe TDD. Nicht deswegen, weil es so ein toller Hype ist, oder weil mich die Theorie vollkommen überzeugt hat. Sondern deswegen, weil es mir schon oft den Hintern gerettet hat.

Gestern Abend hatte sich herausgestellt, das ich es mal wieder versaut hatte. Das kommt nicht mehr so häufig vor, wie früher, aber es kommt immer noch vor. Nicht, dass man mir deswegen einen Vorwurf gemacht hätte. Wir sind alle Menschen, und Menschen machen nun mal auch Fehler.

Das eigentliche Problem war, das ich die Änderung des Verhaltens nicht in allen betroffenen Problemfällen getestet hatte. Aber ich fange einfach mal anders an:

Es war Donnerstag spätnachmittags. Nachdem ich Erfolg bei der Implementierung der Erweiterung gemeldet hatte, war die neue Version auf dem Testssystem installiert worden. Nun kam es beim Testen zu Laufzeitfehlern, unter Anderem zu meinem Lieblingsfehler, der NullPointerException. (Thomas, Du erinnerst Dich? :-)

Mir fiel auf, dass ich einen Fall, der im Betrieb sehr häufig vorkommen würde, einfach vergessen hatte, zu testen. Genauer hatte ich keine Tests für die entsprechenden Konstellationen geschrieben, und deshalb waren die Probleme nicht aufgefallen. Ich hatte mich in der Sicherheit der Tests (insgesamt allein ca. 140 Integrationstests) gewogen und fiel aufgrund der Fehlermeldung aus allen Wolken.

Nun war es bereits Donnerstag spätnachmittags, kurz vor Feierabend, und das Feature sollte am Freitag demonstriert werden. Die ersten Perlen des Angstschweißes bildeten sich auf meiner Stirn. Natürlich nicht wirklich, dazu neige ich nicht, aber ich merkte, das ich ein ernsthaftes Problem hatte.

Nun tat ich, was zunächst jeder tut, ich bekam Panik, und versuchte, auf Biegen und Brechen das Verhalten zu ändern. Aber immerhin hatte ich vorher noch etwas sehr Vernünftiges getan, wie sich später herausstellen sollte: Ich hatte Testfälle erstellt, die das bestehende Problem belegten.

Das genaue Problem war ein Fehler in einem Datencontainer, der, um die Daten später schneller wiederfinden zu können, auf dem Datentyp Short als Schlüssel basierte. Durch die Erweiterung konnte aber nicht mehr Short als Schlüssel benutzt werden, sondern es musste String-basiert sein.

Wie man sich vorstellen kann, ist das Ändern eines öffentlichen Interfaces, das diverse Zugriffsmethoden per Schlüssel besitzt, keine leichte Aufgabe, da ja auch andere Klassen als Client diese Klasse benutzen und es beim Durchführen der nötigen Änderungen dadurch zu diversen Compile-Fehlern kommt.

Wie gesagt, tat ich, was ich früher in solchen Fällen getan hatte. Aufgrund meiner aufkommenden Panik versuchte ich, mit dem brutalen Ansatz „Suchen-Ersetzen“ dem Problem der Interface-Änderung beizukommen, was sich natürlich aufgrund des Maßstabs der Änderungen als unmöglich herausstellte.

Nach zwei erfolglosen Versuchen und circa eineinhalb Stunden unnütz investierter Zeit gab ich kurz auf. Der Vorteil, wenn es spät ist, ist: man ist allein im Büro und kann in Ruhe nachdenken. Das tat ich, und beschloss einen neuen Schlachtplan:

* Compile-Fehler außerhalb der zu ändernden Klasse und der Testklasse ignorieren
* Nur kleine Iterationen
* Sobald die Klasse und die Testklasse selbst wieder kompilierbar sind, sofort alle diese Tests ausführen, mit anderen Worten : Red – Green – Refactor

Nach einer halben Stunde hatte ich die Änderungen an dieser einen Klasse und ihrer Testklasse durchgeführt. Danach machte ich mich daran, zunächst die Compile-Fehler in den benutzenden Klassen zu beheben, und als alles wieder kompilierbar war, checkte ich ein.

Dann führte ich die Integrationstests aus und stellte erfreulicherweise fest, dass „nur“ circa 20 Test fehlschlugen.

Als ich dann letztendlich alle Fehler behoben hatte, und alle Tests wieder grün waren, waren insgesamt vier Stunden vergangen. Wobei ich zugeben muss, dass ungefähr die Hälfte der Zeit unnötig war, wenn ich Ruhe bewahrt hätte und wie immer vorgegangen wäre.

Nachdem ich eine Nacht über den Vorfall geschlafen habe, kann ich drei Schlüsse ziehen:

1. Wenn man unter Stress gerät, neigt man dazu, in ursprüngliche (und in diesem Fall schlechte) Vorgehensweisen zurückzukehren.
1. Ohne die vorhandenen Tests wäre ich nicht in der Lage gewesen, so vorzugehen, wie es letztendlich nötig war, um zum Erfolg zu gelangen.
1. Den Vorteil von TDD sieht man erst wirklich ein, wenn man während der Entwicklung in ernsthafte Probleme gerät.

Übrigens: Wäre dies zwei Tage eher passiert, hätte ich eine hervorragende Geschichte für den Vortrag vom 19.01. gehabt, aber dies ist halt keine konstruierte Geschichte, sondern wirklich passiert. Dann hätte ich aber wahrscheinlich auch den Vortrag nicht gehalten.
