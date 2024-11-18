<script>
  import { onMount } from 'svelte';

  // Sample events data
  let events = [
    { title: 'Music Concert', date: '2024-11-16', description: 'A fun music concert featuring top artists.', image: 'https://th.bing.com/th/id/OIP.Q54huclTbxjxIuVONRUj7wHaEK?w=261&h=180&c=7&r=0&o=5&dpr=1.3&pid=1.7' },
    { title: 'Tech Conference', date: '2024-11-18', description: 'Join the latest in tech innovation talks.', image: 'https://techreport.com/wp-content/uploads/2023/09/Tech-Conferences.jpg' },
    { title: 'Art Exhibition', date: '2024-11-20', description: 'An art exhibition showcasing modern art.', image: 'data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAsJCQcJCQcJCQkJCwkJCQkJCQsJCwsMCwsLDA0QDBEODQ4MEhkSJRodJR0ZHxwpKRYlNzU2GioyPi0pMBk7IRP/2wBDAQcICAsJCxULCxUsHRkdLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCwsLCz/wAARCADqAVgDASIAAhEBAxEB/8QAGwAAAgMBAQEAAAAAAAAAAAAAAwQBAgUABgf/xABQEAACAQMCAwQGBQgGBggHAAABAgMABBESIQUxQRNRYXEGIoGRobEUIzJSchUzQmKSosHRQ1OCsuHwJHO0wtLxJTRUY3R1k7M1REVVZYOU/8QAGwEBAQEBAQEBAQAAAAAAAAAAAAECAwQGBQf/xAAvEQEBAAIBBAAFAQYHAAAAAAAAAQIRAwQSITEFIkFRYaETMnGRsdEUM0KBweHx/9oADAMBAAIRAxEAPwD5Pg+NSDipC5PMeef4VfRufXVgBncfDvqqhTmr6T35qqpsD16jNFwoGOvM/wCFVQ8VIFdmozvQiwFQwq6kVtcK4FxHjBljsoGmlWIyaVxsu+5zVjTA5DHPP2v5CqYKk8gCMjPXypy5tZrV2imjaOVCVdGBVlIOMEHrSxyQAeQ5eFSxgxacQv7JLtLSZoVvIjb3JQLqkhPOMsRnB60tk9wrsV1BBUEk43767+FdtXGg7aoqK7NQRUVOKvHFJK4jiR3kbkiKWY+OBQDosNvc3D9nBEzt1xyXxYnYVs2vAXyr3jaRsexiOW8ncbD2e+tuKGKFFjiRUjXkqDA8z1zQZFpwOFNL3bCV+fZpkRjzPM1tqOQ6dPKuC0eOPcVFQkZo6oaKkYFGCAVWS2hq7QaZ012jwoFdBNEWI0ysXhRkiopZYTRhEabWKr9mKISMZqNJpwoMcqGy+FAtpNTg0QrvUhaCgBq4Bq4WiBKqKKpogBq4WrBaCmDXYNE01GKIEQaoQaORVCuazoB3zXUTTXU0Pju/d8BUdeQ28KjPOuyK02IGPh7q7VVV0lkDMFUsoZiM6QTuaZvLZLV41W4imDprzGc6RnbPn0psAyKioJqCaC6sVYHbwz3ivY+hHpPbej1/PJeQu9peQ9lK0IBkiZW1B1UkZHPIz8sHy9zwzidrb8PvJ7crZXyl7O4Vkkil0/aXUhIDDfIOD4bUsGIzvt4d4oPWemnFeDcZ4tLe8MV1heKLW0iBDJKAQ0gUbgHbn3eNeTbkR8q7WD19/KoJzVtEZx/jUV1QTUHZ51XJqe+ooOqVBJCgEs2yhRkk+AG9aNnwi8utLsOwgP6ci+uw/UQ7+/Fem4dwu0t2Ahjy5+1K/rSH29PZU2rG4d6OXd0Ve6Jgi56AAZmHyHtzXrbXhFpZxaIIlQHGo7l3I6s53NaMESRqO+inNGWNcWpT1hSunvrekTUCD8azZYdLe2ilkTlTcaVVI+VNIuMURKrV8VYLVtNBTSKuEq4SiKtBCRjuphUrkSmFWqKBa4pTAQYrilQKlaCy04VoLLQKld6sFoukVIWqigWiKlXVKMqCqBBDU6DTASp0UC2k1XBpgpVCoqIBio00UioxQBK11FI2rqD4Zk12TXrW4Pwtsk2wH4XkHyagtwPhrchMn4ZM/wB4Gs7beZ1VwbHT3V6E8AtOk9wPxaD8gKEfR8fo3RH4ogfk1DTE1VKkk5wD4VqtwC4H2LiJvxK6/LNCPBeJL9nsW/DJj+8BV2FhdXMcMlos8y20jLJLAJG7F3Xk5T7OR34oWumW4TxRf/l2P4XQ/JqA1lfp9q2uP/TYj3imxXUK7NUZJU2dHX8SkfMVXPjTaCE1HOiW9tc3L6II2duuNlXxZjsK9LYej8CBZLj6+Tnpx9Sp8uZ9vuptWFZ8OvL05iTTEDgyyZCf2epPlXo7PhFpaaXx20wx9ZIowp/UXkK0uz04XAAAAAAwAO4AUxFDqqbC+nvrRsYgq6z1O3lQ3gx0p2JcKijuoGEUtRezqUXAFGArTGyzR0tLCD0rTKZHKgvEe6jTLWPFGVOVFZME7VdE5VBCR0URHuo8UWcUwINuVUI9nirKlNmEiqaMGgqoo6LmhgomnWwGonTsxJI5gBQTR0YkApDO3iwSJT7ZDq/dqKvpFcVq+i7wG0QxKeRIkmPLoW0r+7VIjKzXCyPr7KUIp0IhwUR+SDHWps0Gy0Flp11xQGHOqFdNWVKJpoiJ4VWUKg2o6x1dE8KOqeFUBCVBSmtFQUoEmUUJlpx1oDKaBYio00YrVdNEDwAK6rkV1B4bRgVUrTZj8KGU3rk2W0nurtB/5UyEzRFgJxzopPsqnsj3GtNLQnG1FFmR0qoyewY9PhXGBq2OwFQYRQYjQMeY2oD2sLfbhjb8SKf4VuNCKA0A6CikLa2RSEiRUXPJFCj3Ct6GBVjUY5ClrOD1ie41qqmBUSs2aAZBA86atYMjGKKyAmtCxgzjatRCEsGkjauRMEGti7t8LnFZgGK0CoCcUcLyqkK5poLyqxioRKiSLamkSrOgxUaY0keDXRoSRTc0ddDHuKKZtoeW1PC325Ve1iyQACT3DnT7xrGhklKxIoyzzMsageJcgVUY8kOAdqUZNzW1LGpBI3BAIIIIIO4IIrOlTGailAjGa3A3JEwwOZ3j2pX6TPcQa7ScagZF1xj1Sy5Ut34HPn0rTgT/AEm28NR8PtJXy2w4qLeSSzuZ53t5Gmi7NXT7UjsSwZiMYPLpRqNvg15E/pKifTrq5DniVlai5kklAjtm1iTWxOXbfoBjrnavawj177l/1pvgiCvm/o9A0XpbbGCYSwI98DJErNEQ1s2cNp0gZAAPXFfTIgdV3n/tMmOmwAHKpVVegkUw1CIpGQwtGRKhRTMSDqa0yskeKOEqVCCr6l7iauhQJUlKuH7gKh5CiO+2EUsQBzxvjNNBWRMUswrQmAycUm457UCzCqEUU1QjuqIoQNq6okOlJD92N29wJrqPPy8vZZK889v4UuYN+Vb7weHwpZ7fyrFj1MlYcHlTkUGSNqMIdxtTcMW4GOfhUkFY7fYbVdoMdK0oYduXwq7wbcq1IMRoR3ChtF4VqvBvyoZg8KDIaLwoTRDuNarw4pd46zpS1vGAT508iZ2xQIl3PnWnaRGRsVUIsmG5VsWEYwu29J3MRjlIoF1xC4sZLFI5NAnB/RXOVyc5Iz0x/wA6W69knd6bt5F9Xy6V58r64XvOPjXpL+8sVVgZCSFJ0xoTjG25OBWAw+tUj7wI9+au9+i42e2NHxiZpCqRIil9KEkuzb4OTsPhXS8R4h2nDdU8hWXi/D4vq2RF7KSXQwKqN+mRWOsVxJM8JeKKKeSe3aVywKSu+lQxXlnJ07jcU6YnSXhtvKQZ4OLcGkuGVCqdot2ItCE9Mg/5Nefuvd7eq4SY7096i42xUlc9KKVw7D9Y/OraK9LyM6WPeszjFzd8O4XdXdmYxcJJaxo0sayKolk0MdLbZxyrclTesf0iTPA77blNY/8AvCs53WNr09Lxzl58ML6tk/V5IcZ9JrpCJuMXyDJBS2kFsnli3C/OsLilvJLHLJK8sjCORtcrvI2QpO5ck1t2sGYuKSmaKNEsmmRpM7zQyRt2Yx94HA7/AGUG6VWtrwY2Fvcc+hETHavyryZ7mW39HvRdJlhnwY4SWf2fV7ED8l8I2/8Ap1h/s8dAmXfl1pmw/wDhfB//AC6w/wBnShyjJr9d/MKRklgtjHLM4jQh0BwxOslSBhAT8Kz4V9HYP+p8J1EknVBw1QSSckl5ADWuWlVwEZVyrH1o1c5yAMBtvhQZbpUOJeKxxkkDT29lAck4xjAas1qAC6vXAWDhN3p5YkeGFfcM0SH6UFkNzCIZXlkkMYbXhWO29BS7sLx2jh4kbpwut0hvpJNKg6csImA57UWHCqwBOBI4XJY4AOBgsc1ItENUxVjUYrcZSo+dNIMUBR86ONq1GRM1INDyN87ct/hXK6NkKwJBIIHMEY2INUFBqs+9vPjnpA95C1ANdIR2bDvaFffKlEEl3J8zSkgxmm33z50rJ1rKlz5VQjnRCaGaIWujpt7k90T/AB2rqHxA4tLjxCr72FdTb8H4ny9vJjPwI9ynW24iNs+tY3P8FoD3EXWG99tldf8ABQpr/wCjrIxSSTQN1R/WLcscifM4NYF16VSxyr2UcZRXj7YMXB07gqM5wd/EHT44rHdt9F2V6OJ4ZndF7QOio7JLFLE4V8hWCyKNjg+6luPfSIOB8Ymt5ZIpYoEcPExVwolTVhhvyotjKZpeHzOWMlzwWCSQsMFminYaseOrNN8Tt2uuFcZto11SXHD7qKNfvSGMlR7TirGXkoLf0vGoLdcSJbs+yxxEgbsCTu/ca9n6PyXM/CLdruV5bmOe+t5ndtbFobiSPBbwG1ebg4jcRy29k3CuJ/TjaQymBVgMzqiaWlCl86NueK1+F3sXCeFXlxxZZLGN+L8QkijuV+vK3MhmRQiZyftcu41nHG432783NOST5ZNfY1xbi3B+Di2biM0kS3LSrCUgklDNGFLA9ny51kn0y9Dul9P/APxXP8BWd6VcR9FONQ2Uf5SaT6HLxCULaACSSQRRxoqmUacO2MHwJ6V46x4SLsKZYb5IWaaMXNtCJoFdME5diqHuPrDnVyzxx9sYcdy9Pev6XeiB5X8h87S5H+7WjG0NzDDcQMHhnjWWJgCNSMAQcHevn8PAOCSCIvxDiKk57RFtYNjyKh1dlyD13r3fAo1Xg3DkUsywpJAGYAEiKV4wSO/YVicmOd1GuTiywm7BFTc7d1ZEnGeNQcYmsLN4YkR4wrfR0kcB4lfLGTUOvdXoVTc152S3RvSW7Zp0hVksUZ5Q+gNJEpXDjYNlfV9tTly7cNw4cZlnqosuIcZuuMxpxC7kmSXhklwkZKaAwkjw2hFABxnbHXx21eJWPE7prCW0h7eOOOaGeNN5dTsroyjHIYIJz1rIjaJfShIoy3Zpa3cMKmNkCqI4yypqG4BHPf416iO9a1urVQxIlQjs8ZTGsAsN+e4599TDK3jlydM5MOX5GXxO5vILuftQFt2uZrVGO/2yCNXTc6seVNv9qMnuQn3CmeK8Pa8eUrcQmIyLMUOWfWOWrTtjYgedLOMGMZB0hVyOuNs10x91nmzxzk0wuAu68Xu4Y4Vn+k38sUva4aNDGzkhUcHOQVx4+NaPpCeG9tFc28CJN+VuHW9yE0+tJb3NuFcjP2hkhvLxrM4TcWlpxziEszCOUTXos2AY9lOjsyyTaTnszkg7c8Hps1xySBkiaG1SyQ3du9wjlXeK5S+hR8OpIOrCg79Ad968tms3aZW47v0e5kXE0g32dvnViNqmXaeT8TfOrYyK9rwlJBWT6Qxl+CcQRftM9pjz7Ub1suNyKz+Lrnhl0P17b/3Aazl+7Xo6W658L+Z/V8/szJFbyhtJmuuzsIlydJ7R2BkYc8Ky+3BHWpv4XVOKnQUj+j3gUZBVsIwDKAevM+fhUh5FvFt2cLFDmeEAbiSQ4bcDrjPPn50fiBzaX2ST/olzz57QtX5P4feYW59XnlfpP+P7PpPDt+F8H/8ALbD/AGeOpkG5qnDz/wBGcIH/AONsP9nSrSGv2I/nl9lyAZ4QeR2PtdRXyedLO5v+OPe25aaW7me20QhdZNw0R1HlpABA/jX1Vn0zRt90E93JhXh7z0Ou5priaDiEEJmuDLo7OYx6CzPggMNyTk7gc+/bM9tQb0fitoeNcajhhtojDbLDIsJKhmE27QxdI9t8k78gBXqY/sn8cn96vP8AA/R+54Rd3d3Pex3LXFtFb4jhaPGhw+olmOfdW/Ecp/af+8aT2USu/wAKipFaYFSiZoQqc1qIswDDBzjw8sVRYIllEyZDadBA5FeurPXl7qvmpzVBM1VzkRjvlh+DZqM1zHJh/wBavwVjQGJzQHopoL1kLtQ++itQj/GqjO4s+m1059aVwE6/Z3JIrqQ4rMZLnswfVgGj+0fWNdWK+T6/qrlz2T1DjNw85xa8WOonb8nSdd/0iKBJDweTOvhfEX1Yzq4eMnB1b5fvrcLVTNTUfX7rNtihubJbe0u4YLe0u4mNxCsSgO8Loq4YnOzVsIeRwdiNmHTNBFXB5Aczt76qPlvEJZU45xJJJrszfT7+2STtplZUaUlEjkV9WO5eVbd67t6ChJZGkktuLm3Z5C0jHsrmYAszhuhHM+HhVOOcA4lN6SQyQdr9DvZ7e4neGUDsZJCY5WXfAIAyPOtyfgljHwW94Kt047ac3KXE5ecpOZFk1Ex7nOCDv1p/q/DpbO2fd8offOAADjY4zjoM7b99e49Ebvs+HOnYXUxjupRmJAY01qr4GpgM+z51hv6MekRY6ba2CqWwwukw25w2GJPLFe29F7McFsJYLqRDcXM5uJBGhwmEVBHqzvjHPA51y5uPHmx7a1hn2XYj3DvkR8NuhnOQI4lHf96tHhCSx2WiSIxn6TduEc7qkkhdRt508W8ajVXPi4MeK7jXJz5Z49tTpFeY4jL9F9IYJg5XtLa2YkAYjEesFzkHcjKg8xmvTg86Sn4VY8QuopZ2mSRE7INEwGVzqwQQRtv0rpnj3Y3GufFn2ZTJ5tLlJvSayde2IJvFiMzqSYja9AB95T7Md1epEBne2cPp7N4+0xkMYVcSaVK786Tm9GuH2d7a30d1dSSw6iok7MKdUbR4Okdx+FaELYI86Y46x7avJnvPuix1rPMzcnjfRyxpEuQNu7fFJSH1yem3zp2TswHYKoZvtMBucd9IOTW9uTzUj3EHEb3s40IuZpo8khSrYfDZO3Xbz7zU3YkPDbyeQ9pcF+GjPMOUvIVZ9IOkFjlj55o91EWuOIJINCzNKFZ8jUjbgqeflQ+JzauF3K4ETskMj9ngnMc8eFwNt8fGvNb83l6p+74fRbyW5V5TbxpI6EnTJqxIM8kKkb1l2/GbsB1dIXYnI1KwIA2wAhFabNlyfGsLjCCKS3niAVpWdZO4uMYb29fKvTb9Xma8dx20VvNo3nVXwobSueeTv86BxY44ddfjt/8A3BQ+GvmzgGd1LptywrHar8TV34deBEZ2URy6UxqKxuGbSCRvjp1pbvGuvDZOTG37z+r5rO7SX06YypWISb49RSWIyN6ZvJHa2v8AVgA2twqKBjGIWz76XaG8W8nla1uVRjkMY984C40g52qbmRjb3I7ObU1vOoHYybsY2GBtX59wy35j7vg6jgvJnl3zzr6/x/6fUrOaJOH8IQsNbcPsQik4J+pjGw51eRhQbNmFhw1TqBFjaBgdsEQoCCK5zX6O38/LXILlQLk27DPrCJpNS92ADSbI3/3h8/8AhXH+5TU8hRbmQbdlaXEozkjMaM4yPZXgLX064k0sX0uCy7E6jIIkljkIA/R1SEfCpq1qPZMvfxuQbf1Mg/3KZhGiNF7YzaQfrSMF8knJFef4P6QXvE765tZYLaOOK2a4R4DNrYh0QAl2KdTnHdW9F+bTxB+JPfT0UZXRs4IyCRzB5ddqItAUKOQUeSgfKig4rW2VndUV3c4VFLMcE4AGeQ3pP8r8L/r2/wDQn/4abzXam7zV2hYcY4R1uffDP/w1P5Z4L/2xPbHMP9ymg3LepJHUDHiBV2FBxnguR/p0X7Mv/BRob+wupIUt7mOVgXchNWQoXGTqAq+EPNEI8UX+VSqQo2pIo1bBGpEUHB6ZAqbDBZQMsQB30NmQgFTn2GqPqbGJHXGc6Qhz5hwRVBqUnMjPsBusYx5aFFXaIY0GWRYo5ZWxiNWf2gbD34orGsji9xphSEHeU63z9xOXvPyo4dRyfseO536MR3Z3Z2PrMSx8zvXVTNdUfCZW27eu1V2aEGqQay/oQwNSG3HmKGDXCisORQrygAA636D7xo8G1uQekhx8KrMPrptv6Rvnmpj/ADUo/WB+FRV85qGVW559jEfKqA1bNQa4Oy+Q+VTmhocxxn9RflXE0BAd6lGKyA0rLdWtsjS3M8UEKFQ8szBUXUQo1E952omoHDKVZTyZCGU+RXIqJoxdSawKWRq6R8ihK2KoO77GlGOaKzZFBJoo0UFrOR20YfbGSzDb+yRTD8G4QykqsyE/dkDb/wBsGloTgjenxLtTwGQ/rGpbQwwyqw54YA499Kq+9G1eNUWUInqoqqMk4UADPsoqmls1dWoGw57z76gkHmAc9CBQNVdqoLs3yoDtUs1BZqBe8YC34iScAcOvSSTjH1MlfIuGrLIz9jJZ5mjjjk+laTssiyeoSDjJAzX1m5lCY1RSSo6lHEaB9t8hlPQ1jPaejLFjJwm1jY9TZLE2fOIA/Gky01CHo1cwS3t5CqKGitSzNH9g5lAOPOvVRn1I/wAIrGs4PR2yklmskhgkkjETlTNhkB1AaXJXnWrGwMcWCCCgIIOc9am90pkNS/EL6Ph1jdXsi6xAq6EzgPK7BEUkdO+rg1nekPZngnFA4yNNvp8H7dArez+NL6dODGZ8uON9WvOD0m4zcBgbhIQRlgiJFgE4HZFF1/E01a+kV3aBmubuO6iH9HMr9v3+pKBn35rz6QBwVWIAnSwlydXdpwDy5cxQZWtF+pxJJPrBDFh2aoAdQ0jv2xv08a8m7v2+/wA+i6ecfbcJ+n/r2K+mMBGfoQywYoO3Zjkf1g0D4GneFekkHEp/oskQhuCGaLQ5aOQKMkAkZB69c181kkIlIT1VTbbnkjetP0fmn/LPCFVsgzBCCB9kJJn5muuOWW/L5zrODpcJljhh/vuvqeasGoIblU6q9L5wbVVGNVLVQtVRJJrzPEZ+2upSD6qfVpjuXbPt3rdu5+wt55c7hdKfibYfz9leVJyTms1+F8X5vE4p/GpFdUA11HzmnpRKvT5VYSeB+FACN41dYvOsbf0IftDg7D31yu2267+dVEQ76ssTb/Yx5mmwjMuZZT3tVQMJJ4j5U6VYs2TGMn7o/lQpwBGfXyR0AAHKrAkDVs0EGrg8qg0Y2Bij2P2e84+dW9U/oj25NAiYCNPVBPrc/OrF6wrpI4JUeN40KMMMNI3HtFJPYIjI9tI8LIjIOxdohgnP2R6nwpzOcb1Uncc6aUobnilvgOUmQf1iaWxy+2m1ETiUW/axSx+Iw6+8UdmI6GglI23K4Peu3ypuwMJcW8o+rlRs8hnB9xwa5s/499Z8lorg6QhPTI0N+0v8qXYcRgd2jmk0sEwkigxDAxsBt51e77mm0hxTAfasGPiVyn5+2DD70DHfxwc07FxOwkwO1KN92UafjuKu4mq1EfemA21IxsG9ZCrL3oQw94plTtWkF1VYNQQakGgPqri9B1VGqguzUJmFQzUFmoqS1ULHvNUJquagllifZ442/Ein+FSCAFAAAAAAAwAB0FVzUZqoODS3E+xbhnE1nYLG9rImT/WEZjAHfkDH+FEBrB4/cTxyRStEr2dk1uQkw+puJ5QXKlcjIAwDv86zl6ezouKcvPjLdfV4+Wa4BWNndSEUMCTg7ggHHShK0kjnChdKnJX+NWum+kESRglgcHSMAjPLHhWhYQcMt0me+lkOmIuI40ZnuJT6qxhvsqoO7E9NhknbzzWn2GOGV5LjL8k+rGJJZic5zjlua2PRzSvHOFHl9ZKPW7zE42rOmZGz6oz0OMEUxwU44xwb/wAbEfeCK3i/E6zGS5SXb6qG5VOaCGq2qvU+dELVGapmu1DqQBzJJ2AHMmiMrjM+8FuOmZX9uyj/AD31jk0S6n+kTzS9HY6PBBsB7qAKy+N6zl/a81y/kuDXVUc66jxV6zTtUZxtV8qM1UutZf0F2pqgs/fUaxXaxUUGTXq586HokORudjyBJ9wq88yJpLEDOag8WexuI7aOzM6aWNwip2hu9SDASRfUCblWBIOR1FIsmyoiiRLiSe4jjWFQziMGaUZOkZjGD8agK2hJAkyq7Mq9tHoY43BG5GD51orc8TeSaWe5hWGSOOO34dbKhgslQDcyDm5wM7nGPcG5nnkVjI5YqBjcAKAegFaa1NOg09mNXPUe+rnT0Ue3b5UvA50ksQQckbnPnvtRllQuqlVwds5JrlcptJjdJBbOAFooQkDUBnpip7RV2AA8gKrK76GKjJXfCkaseXP4U8pFniwooOgVdbuKSPSG9YZBGVPLxG1U15P86m104qO73VXAqxO3OhFwOdUT2MLblAD3rsfhQ5LGFxghWHdIu/7Qogcf5NX1eAqDP/JzRNrgmmibppbUv86OlxxyADIiu0HQEBz55wf3qaDVf1DzA8+R+FVQE43bAhbmGW3f9cHGfDOP40/Fc2s+OxnjbIzgnS3ualWiV1KthlO2JFDA/wCfKkpOGWhDFEeBvvW7YHnpOV+Fa7k1G4SQaqWrDkXi8HYfQ7oMiau0jkdgJB09Ugrnvrhxi6g2vbJ1A5yICF/aXUvyq9ydrXZjvQyaVi4lw+4GUmA/HgD3jaj5DDKkMp6qQR7xV2ariarmqk71BNVFs12aHmuzQFBrD9IGuTBcJAHbVHboY0BcyGSTSoCYJ1c8Y7q12kjjUvI6ogGSzEAfGsfivGYraN3tGX6QYmCSFWIBGUGkct98V0kkxtpxzLk5cceKbs8/yY6cNtuHxCXi2trh8dlZW0oXSvM9rIoJ9gI586SvbmK8ZGgtILRIgYtMI+0q8mkydz0z86DruJT28sju7qJJHc6i2euTQbkFMAZBmGpdsZ6ZAPfXkysviTw+sx48+PCcmeVuX6Tf2gGcknOeYp7hDKvFeDt0F5EN/HIpDdQBpI8xTNk2i94a+MkXtocD/WqKTw/O5fMs0+pA1OqqyJLFI6So6OCcq6lTzO+DUZNel+EJmqsSQdOM4PPH8ajNRmptGBcWssbNrQoMnH3TnuIpcqRXpyQw0kAg8wRkH30rLYW8mSn1beH2fdU08XN0PDy+bNX8MEV1PTWE8eSU1L95N/eOddTb8nP4TnL8tjUkn0YPPPvoRuXPJTv386iUDMbbkA4x4nvoRVjk+J61i19PIJ20x6AVZZWw4Y6vs8sDGRS2h88xv3Zp2KPGO8oDvTZrRG6JKKe5vOkihJwAMeNeha3aRHEaFjtsqljz7lFZM1vNHJKjBlZWKnUN1Pcc71Y1iNahlhKDGVORjlvRPUkEsYKltJ1aDq09QMjautEKo4bcY38abhiQyQoAArOiHwDkKarUuiSl1GAD0A37qle0LxkjHrV6kej9gNme4cA9HVM/sLmjLwfhaEEWyMRuDIXk/vkj4VznF52z3zWnmu0jiPaSq7oudQjxq5EZG/SgxSQywsttLjsu1UO/54esQC+MeWQN8V6i+s4jZ3SxxoCE1qEUD7BDdBXjJ43ift4F+sjDDAyvaEjADEdOozy5111onlwSTsWJXSySBWA5I7dDj/O9NR5Kod86Rmm+EqXuJUZQUkgdnB3BIK4yOVNz8PVBm3AGM4jPL+ya59l9pcvozcHFCYGmmV1ADghhkEEYoEgxnFZqhAHIxt8jRQWx/Kgs+gjbNEWSNh9oKe41NiQ58aIHNDI8cjvqeXjVBBISehqdfdS+a7WfCgOcN9oDz6+wioOpRs48m+WRQw461OQRkY8xQKT2dpKS0kGH5dpASre9MUl9DuIiWtLthj9GTn5akwfhWqTvVGCt9pR54wffUVnG+4tb/nYTKo5lRr9uVw3wokPGrSTIcMh5HB1AHyODTXZFmCpqZzyQKXY+QUZog9Hr28x2ttCinOHuGAI8guX+VblqeL7RHcW8xAikRiTsAcMfIHeiyCeMRkKmWbAWRiGbwAG9PWHonZWhZ5buZ3dQMREoqDqFdsvv13rcteH8OtCzW8Co7ABn3aRgOQaRyW+NejDUm64ZzLK6niPMpi6ZYY1t2fGSAYxjfm5Y9KxePcAZ5IDaxie4nVe17Ht1tYFjGkM7yQhMY5kN7O/6WoRQQqgd+OvXpVtR7yMb7E7fGpl881XXp7/h8u7D2+SSej9/FCZC/C/URmA/KOlgFXJOJoUBx3A79KxpI9Gh3Uu5UEyH1jgjn15190MjY3bOD+lv8681xH0P9GeIPLMIZrO6kYvJPw+Ux5Zjk6omzH+6K4Zce/T9vg+LZ4/5s2+UghnAVC++SBkDHcSa5IZGYOIzpB5DJ5eJr2cvoDOGKJxS3lh1A5uI5llAB6rGSv71aNp6I8HtkH0ueW6kGwVCYIF8kUlj7Wpjx69uHUdfOTeowuF8a4laxxwTl7uwU47C4yXiHIm3lb1lPhnG3LevSho2WOSJtcUqLJE+MEoe8d45GqLwbhNuGCxllHLVJIW94NXKxRqsUbHSu6oTq0554POur8h2c1NVqaCakVFSKqLA11QK6htzQZG+OYNctr5fOndIq4IrjXUj9EAIJGQN8YO+Om1egtoLMxwyxxQBGjVx9WuwO+7Nk1kTvIqqVPUg7e0c6c4RKrxTWsm/ZNrQHqrc/wDPjWsL5StFp0GRHv48l91eS4pJGeIXhkkjRi6EhtC51Iu+GOd69d2UH3B7zQ5LWwlx2trbyEcjJEjkeRYZrszLp462mt5JDbRSLJKVLiKA6nx3tj1RnxIrTtbKcuksrCMaUPYRnUUfJJLznAPQbIOXOtwWtqgKxgxpnOlAqrnyAqRDAu5LHzb+VZ01ckxvcMdnLHrkeqPfVwLqMMWmEpaRmA0BQoY5Cgc9vOpBCLgDCd/Ie0nb40pNxHh0eRJdW48A4c+5MmqwUuuLRpK8Fz2kOS8ehQRrGMEjXjPvpEcLNzGk1txArDJkqstojFcHGCVcGmZ+NcLKlBHLMp5qECxk+PaH/dpFuNso0W9pDGOmpmbA8lAFZ3Gt6N2/DfopZvpTvIVCMcdmuM59VQT8zTJEqjOosO987e2vPycV4jJ/TBB3RKqfEZPxpV5ZZTmSSRzn9Nmb5ms932Z3tv3E9oM9pLCcZ5OCw8gMms8vFLqMRLKOepSCM74xWU80MWTJJGn42VT7ufwo1pdKe0CKzodPrBWXfw1AZFYyy21B5UO2DQShwfPbNHlbIBGdz5H20M7YJ3B2zXKu0EhP1SHzHuNEJWqRY7JccgzDPfvVZG0DbBJ7961GL7W6naoyv/OhK5YE56kcgKnbw9tFWJGefLu3quT/AMq7JqCf41ROpvZ8absbV7xnLMyQxEB2C+szc9CZ+JpAsQCd+XSt23kjtoYYC66wCSDndzhiNh41ZNs2tK3htYF0QrHEOeFGWc+LbsT7aMTjAI3JONWBnrSaFk9Yv65wSRyVegH8amS9jBzJIo0jmxA+ddYhxS++648SasHcYBeMeSsf41lHiUZ/Ns0vrDaJHkJx+EY+NDN9JtmG5AyCS8Eij4A1UbDTOOUi5/1efmaC09yM4nt8dA0Tjp3hqyzfxNkasEcwSQR7DvQnuVYHDbHv93Wg0TPxM5Knh7AHrJKjH9oYoT3PFFAzDbgkDJ+lQ45DJ3IrOabuO3dQWeRs+Ph/Ooh1728G8kDhWYjKgOu360ZIoJvC36WMnOTkDNCSKQsW1ENzyDg0wsLc3cnI5MxPzpsCZ535ZI23ByKgo6+s22SedHZra2XW8iR45knTnyA/lXW17w3iAFtHHaLPlsTcRvJYNWTt2UUa6T5F6SbrGV1PAAq1ao4DOkYBvFklBOovD2aeAGliaVl4dxCLJMJdRuWhIkHuHrfCtWaJdzyVzXV2CCQchuqtkMPYRmuqKsK6uHSuqh3X4e+oDb+z5VTNcSfVP63z2rz11XkUvG694yPMb0vbz/R54Zs+qTok/CdjTAakZlwZo/avt3FJdU9vQS39lDntLqBcdNYZvYqZPwpGTj3DkyEM8v4E0D3uR8q8ryJwO/lQpLiCL85LGp7mZQ37Oc11uVc3o5PSKU57K1jHcZnZj7l0ik5eNcVl/pxGD0hREx7cFvjWKLgvtFDcS7ZBWIoh/tS6R86MkHE5eUMcQO47RmkPuQKP3qzcquhpJ55TmaWWTr9Y7P8A3jVCwUZJAHjgUVOGznHbXTeIiCoPL1ct+9RorCxTOU1tzy+Wz4+tk1i5Loj2yHOnL4P9GrP/AHRj41YC6cnRA3LILsFHuGTWqFhTZVUeyoZx/Kp3HayxbXzY1PHGOuF1H944/dqwsUbIlmmk6kayq+6PSKdYg9d+lDJAO9Z3W9QNLWzi3jhQHwC5Pu3oqbZ226bVKuowcAVWWXuDHx5fOqBynkc7VLYK6hsCCAO48qWldtsY50dWjMRLHccwdhkgjbFYtdMRIiBEDqwS7EgnuPdQHYuSamJgIl8WbOw76NDCkjhdSIT1cnB8sV0k25hoPVGMDc1DaQMsQB3k7Vqx8Oth9tnf9UHQo/Z3+NOwwW0X5uJFPeFBb9o5NdZx1m5MKG2vZt4oJHU7hiAi48GfAp+Lg10+DNLFGDzCAyN/Ba1waKprc45GblScHBeGoVMqyT4IyJXIXy0pgVk3SyQyzGVSWRtDsNwANgfbtXp1PjSN+kcbxylkzcExmNgMuUTcgddsZ8qtkSVlwv2yKrTFUjzjQMysPBm9UD2GmFktoB9TBGG/rJQJpd/1pM/AClbmOCIqbQqucsyZYpnu33Hhil+1fk6vnvADD2Fay00fp8wGNRz1/wAigvfTn9I9e/O/nSwGrq3jhGoqxA/ePkv86IhppZMB8OO5gGHs1Ch9iDkoun8DNz8uXwpnTFGpdyFUA5ZyFUeZO1Z1xxu1iOi1hkuXzgspEcI/tsCT7BUDyRNqAYKfZhv3f5UG74lwjh503M47XG0EOZpvaicvbisWa94zeAq0wt4jkdnaakJH60h9f4igQWMKuEjiLzOchY0Z5GPfhQW+FUPnj8spb6JaMoOMNdYO/foj/i1Ua64tPntLl1B/RhCxqPAad/jWnaejfGJgCYo7WM76rpvXx/qo8t7yK3bX0X4dFhrqWa5b7uexh/ZjOo+1qsxtTbxiWzzuEQTTTH9GPXJIfMDJrTtvRLi9yQZ2js4jg/XYlmwe6JDp97CvdwQ29snZ28MUMf3YUCD243oma1JIhSwsk4faQWaTTTLCCBJcNqkOTnnjl3CmeVST86BdXVpaJ2l1MkS4yobd38EQbn3VdotLFDKNMsccg6B1B+dZ11Y8KiTtZX+jAnYhz63gqNkn2Uq/G5bkSfQYxFChKtc3OnOcfoqTpHxrKmuoFcyOzXc55vKWEY28fWP7o8KGhRJbu7rDIXCsVGsBZCB1KAnFdXnr/wDKc0/0iONXTQoHZGNHGM59TAHlg11cu50mL02uoLmlBMfCqmXxrjto6JM1V9DMGJIOMbYzt1pQS4HwrjJtzxtTYDNwq1mmd5JJ2Rm1CNpZNCk9FVCFx55osVhw6AfVQop71VVOR36Rn413bgedVM5JHdTuDQ0L9lVHkB86hnpYzHf477VXtgeWT5fz5VAdpAMDc0NpScdMbg+NLvKc7Y5Dx+VCeTPl3A/xqKYM+c9MHcZ5Gq9uM957wOftNKF8E7DxNQZOgqBsyknbA89z8KoxLZyzez/ClS/PfbzqBLv/AI1Q2sgXA5E9cbHxFQ8gNLGQMPiMc8+FUMnQnyPKgKxz1qhJwRvtvVA3jVtQINRZRo5CUUbbZ39tFDdKUBxtRYzz38vCuuHtmtS3vpItKvl4/E+svkTWtDNFKoaNgw64O4PiOdeaHxokU0kLBlYq3eOvnXo25vUA0QNWVbcQR8LLhH6MPsH21oBqqG1YbUrxLhy8QSArMYp7fUYm5qdWCQcb9BuKsGxRQ9B5iae9sHWLiVuzc9E8RB1A/ut7wfCqflThf35fIQyZ92P416tjHIrJIiujbMjgMpHiG2rPl4HwWUk9i8Z/7iV1X2Kcr8KzcWtsJuM2QH1cVyx7jGF+Z/hS78U4hJkRRRwrjGpj2j/wX4V6AejnCNz2l2R3dqg+ITNMw8F4JDg/RVkI63DvL7cMdPwqdqbeMWG6vJNzcXcvLSitJpPkgwK17f0b4vKBqjitlON7hwXH/wCuLJ95FeujMcShIlRE6LGoRfcu1W11qYptk23oxw2IqbmWa5YHOnPYxfsodX71bNvb2domi1gihX/ukCk+bDc+01XXU6/Gqg+oV2qghv5UtdcQsbIE3Myq2Nok9eUn8A5e0ilD2qgXV9Z2S6rqdI8jKp9qVvKMb+/FeXvPSa7l1JZoLaM7a2Iecjz+yPYKwmd3ZndmZ2OWZyWZj4k1jv8Asr0d76T3D6ksY+xTcdrJh5j5D7I+NefkllmdpJnd5G+0zsWY+00OurO6o0MmYp1HJblveIkFVAYuS++OQ6Z2OQKDaE6bhjtruZWGdtth18qPqyQB1IHvOKtGnGn1cYK5GkH3711HXGwzy291dWIM3tM+yoL45UNP0q5udcXRbtcV3ajrQDzqx6UFzIe7HnVO0PefLpVW6fiNcelFXDA7nfxPf7as0g07MQaFVR+l51dCzPjrtVNdSf0fI1UdalEFhVC/dyzvtVmqjcqCpYVXUcneuHIeVV61VTrO1TqyN/8AlVD08qjpQF1AePdiuDNnbHzqu2/kauOS+Q+VEWU9+TTER2bel05UaHm/mPnWsfaUwDXGpq3dXdgMMw5cu7+Rp214hJEAAdSdVOcjyzSh50M/nYfFjn3VUeohuopxlG3HNTzFHD+NeetSRLHg/pqK9Dtn3VqItq8auGqgq9BOvxqdfjUVO21EW1+NTr8arU0Fg+dhSt3xPh9lkTTAyD+hiw8nkRyHtNTdEi1vSCQRE5BGxB09DXh+gPUqSfE5rGWViti79IL6fUlv/o0RyMoczMPF+nsrHJYksxJJJJJJJJ7yTXV1ZV3dU1PdXVBFTU1NBGelXhGqWIfrr86rRbf8/B+MUo1g2P8AO9dUpzf+zXVlH//Z' },
  ];

  let selectedEvent = null;
  let isModalOpen = false;
  
   
  let selectedDate = new Date().toISOString().split('T')[0]; // Default to today

  const openEventDetails = (event) => {
    selectedEvent = event;
    isModalOpen = true;
  };

  const closeModal = () => {
    isModalOpen = false;
    selectedEvent = null;
  };

  const getEventsByDate = (date) => {
    return events.filter((event) => event.date === date);
  };

  let currentMonth = new Date().getMonth();
  let currentYear = new Date().getFullYear();

  // Calculate the days in the current month
  const daysInMonth = (month, year) => {
    let date = new Date(year, month, 0);
    return date.getDate();
  };

  let days = Array.from({ length: daysInMonth(currentMonth + 1, currentYear) }, (_, i) => i + 1);

  // Function to handle next/previous month navigation
  const changeMonth = (direction) => {
    if (direction === 'next') {
      currentMonth++;
      if (currentMonth > 11) {
        currentMonth = 0;
        currentYear++;
      }
    } else {
      currentMonth--;
      if (currentMonth < 0) {
        currentMonth = 11;
        currentYear--;
      }
    }
    days = Array.from({ length: daysInMonth(currentMonth + 1, currentYear) }, (_, i) => i + 1);
  };
</script>

<style>
  .calendar-container {
    overflow-x: auto;
    white-space: nowrap;
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f4f7f9;
    border-radius: 8px;
  }

  .calendar-day {
    display: inline-block;
    padding: 20px;
    text-align: center;
    border: 2px solid transparent;
    border-radius: 8px;
    cursor: pointer;
    margin: 0 10px;
    transition: all 0.3s ease;
    background-color: #fff;
  }

  .calendar-day:hover {
    background-color: rgb(206 68 164);
    color: white;
    border-color:rgb(206 68 164);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }

  .calendar-day.selected {
    background-color:rgb(206 68 164);
    color: white;
    border-color:rgb(206 68 164);
  }

  .calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .calendar-header h3 {
    margin: 0;
    font-size: 24px;
    font-weight: 500;
    color: #333;
  }

  .calendar-header button {
    background-color:rgb(206 68 164);
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .calendar-header button:hover {
    background-color: rgb(206 68 164);
  }

  .event-list {
    margin-top: 20px;
    width:500px;
  }

  .event-card {
    display: flex;
    align-items: center;
    background-color: #fff;
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 15px;
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .event-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  .event-card img {
    width: 60px;
    height: 60px;
    border-radius: 5px;
    margin-right: 20px;
  }

  .event-info {
    flex: 1;
  }

  .event-info h3 {
    margin: 0;
    font-size: 15px;
    color: purple;
  }

  .event-info p {
    margin: 5px 0 0;
    font-size: 14px;
    color:purple;
  }

  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    z-index: 10002;
    width: 400px;
    max-width: 100%;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s, visibility 0.3s;
  }

  .modal.show {
    opacity: 1;
    visibility: visible;
  }

  .modal-content img {
    width: 100%;
    border-radius: 5px;
  }

  .modal-content h3 {
    font-size: 20px;
    color: #333;
  }

  .modal-content p {
    color: #555;
  }

  .event-tooltip {
    position: absolute;
    background-color: #333;
    color: white;
    padding: 5px;
    font-size: 12px;
    border-radius: 3px;
    top: -30px;
  }
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent black background */
    z-index: 1001; /* Ensure overlay is above all other content */
  }

</style>

<div>
  <div class="calendar-header">
    <button on:click={() => changeMonth('prev')}>←</button>
    <h3>{new Date(currentYear, currentMonth).toLocaleString('default', { month: 'long' })} {currentYear}</h3>
    <button on:click={() => changeMonth('next')}>→</button>
  </div>

  <div class="calendar-container">
    {#each days as day}
      <div
        class="calendar-day {selectedDate === `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}` ? 'selected' : ''}"
        on:click={() => { selectedDate = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`; }}
      >
        {day}
        {#if getEventsByDate(`${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`).length > 0}
          <div class="event-icon">★</div> <!-- Add the event icon (star) here -->
        {/if}
      </div>
    {/each}
  </div>
  <h2>Events for {selectedDate}</h2>
  <div class="event-list">
    {#each getEventsByDate(selectedDate) as event}
      <div class="event-card" on:click={() => openEventDetails(event)}>
        <img src={event.image} alt={event.title} class="event-image" />
        <div class="event-info">
          <h3>{event.title}</h3>
          <p>{event.date}</p>
        </div>
      </div>
    {/each}
    {#if getEventsByDate(selectedDate).length === 0}
      <p>No events scheduled for this date.</p>
    {/if}
  </div>

  {#if isModalOpen}
    <div class="modal show">
      <div class="modal-content">
        <h3>{selectedEvent.title}</h3>
        <img src={selectedEvent.image} alt={selectedEvent.title} />
        <p>{selectedEvent.description}</p>
        <button on:click={closeModal}>Close</button>
      </div>
    </div>
        <div class="overlay"></div>
 
  {/if}



</div>
