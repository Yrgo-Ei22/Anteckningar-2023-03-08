# Anteckningar 2023-03-08
Implementering av drivrutiner för Watchdog-timern, som möjliggör systemåterställning eller 
avbrottsgenerering ifall ett givet program fastnar i en loop eller dylikt.

I bifogat program genereras Watchdog reset via nedtryckning av en tryckknapp ansluten till pin 13 (PORTB5). 
I programmet aktiveras Watchdog-timern i Interrupt Mode med en timeout på 8192 ms. 
Om Watchdog-timern inte återställs var åttonde sekund genereras därmed ett Watchdog-avbrott (avbrottsvektor WDT_vect).

Efter fem Watchdog-avbrott låses programmet och en lysdiod ansluten till pin 8 (PORTB0) blinkar var 50:e ms (via timerkrets
Timer 1). Det enda sättet att låsa upp programmet och stoppa blinkningen är att genomföra en systemåterställning, exempelvis
via reset-knappen. Meddelanden om antalet passerade Watchdog-timeouts, Watchdog reset samt systemlåsning skrivs ut i ansluten
seriell terminal via USART.

Filen "Arduino drivers.zip" innehåller drivrutiner som motsvarar Arduino:s funktioner pinMode, digitalWrite,
digitalRead samt delay. Drivrutinerna fungerar identiskt med motsvarande Arduino-funktioner, med skillnaden 
att de är mycket effektivare sett till antalet klockcykler som krävs för en given instruktion.