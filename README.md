# email-scrapper

##CODE : 

```
def f(email_id, password):
    import imaplib, email, os
    user = email_id
    password = password
    imap_url = 'imap.gmail.com'
    arr =[]

    def get_body(mssg):
        if mssg.is_multipart():
            return get_body(mssg.get_payload(0))
        else:
            return mssg.get_payload(None, True)

        
    con = imaplib.IMAP4_SSL(imap_url)
    con.login(user,password)

    totalmail = con.SELECT("INBOX")
    totalmail = totalmail[1][0]
    totalmail = str(totalmail)
    totalmail = int(totalmail[2:][:-1])

    arr.append(str(totalmail))

    con.select('INBOX')



    #TO loop in all mail
    #totalmail - last mail number
    # for i in range(totalmail, 1, -1):
    #     result, data = con.fetch(str(i), '(RFC822)')


    result, data = con.fetch(str(totalmail), '(RFC822)') #latest mail

    raw = email.message_from_bytes(data[0][1])

    # print(type(raw))

    x = get_body(raw)


    email_body = data[0][1]
    for response_part in data:
        if isinstance(response_part, tuple):
            msg = email.message_from_bytes(response_part[1])
            email_subject = msg['subject']
            email_from = msg['from']
            arr.append(email_from)
            arr.append(email_subject)
            arr.append(msg['date'])

    arr.append(x)
    # arr = [last_index, email_from, email_subject, email_date, email_body]
    return arr

print(f("xxx@gmail.com", "######"))

```
