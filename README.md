import string
import urllib2
import time

emailList = []
attempts = 0
total = 0
skip = 0
gmailAddress = "sapanpshah24@gmail.com"
inviteToken = "https://oneplus.net/invites?kolid=Q5SD3P"

def insertDots(str, at):
	if at == 0:
		emailList.append(str)
		return
	newStr = str[:at] + "." + str[at:]
	for i in range(at):
		insertDots(newStr, i)

def allDots(str):
	for i in range(len(str)):
		insertDots(str, i)

allDots(gmailAddress.replace(".", ""))

total = len(emailList)
print "Count: {}".format(total)
print "List: "
print emailList
print

for email in emailList:
	attempts += 1
	if (attempts <= skip):
		continue
	requestUrl = "https://invites.oneplus.net/index.php?r=share/signup&success_jsonpCallback&email={0}%40gmail.com&koid={1}&_=1438677876942".format(email, inviteToken)
	print("Sending invite to " + email + "@gmail.com ({}/{}) ...".format(attempts, total))
	req = urllib2.Request(requestUrl)
	try:
		res = urllib2.urlopen(req)
	except urllib2.HTTPError as e:
		resCode = e.code
		print("Request Failed: {}.".format(resCode))
	except urllib2.URLError as e:
		print("Request Failed: URL Error.")
	else:
		print("Successful Response.")
	time.sleep(15)
