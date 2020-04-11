# virtual_assistant

import wx

import wikipedia
import wolframalpha
from espeakng import ESpeakNG
import os
from gtts import gTTS
from wx import App

text1="Welcome User! Hello I am Pyda the Python Digital Assistant. How can I help you? "
text2="Here is the answer you searched for :"
language="en"
myobj1 = gTTS(text=text1, lang=language, slow=False)
myobj2 = gTTS(text=text2, lang=language, slow=False)
myobj1.save("welcome.mp3")
myobj2.save("Answer.mp3")

app_id = '26Q368-9Q9ERTXHP6'
client = wolframalpha.Client(app_id)


class MyFrame(wx.Frame):
    def __init__(self):
        wx.Frame.__init__(self, None,
                          pos=wx.DefaultPosition, size=wx.Size(450, 100),
                          style=wx.MINIMIZE_BOX | wx.SYSTEM_MENU | wx.CAPTION |
                                wx.CLOSE_BOX | wx.CLIP_CHILDREN,
                          title="PyDa")
        panel = wx.Panel(self)
        my_sizer = wx.BoxSizer(wx.VERTICAL)
        lbl = wx.StaticText(panel,
                            label="Hello I am Pyda the Python Digital Assistant. How can I help you?")
        my_sizer.Add(lbl, 0, wx.ALL, 5)
        self.txt = wx.TextCtrl(panel, style=wx.TE_PROCESS_ENTER, size=(400, 30))
        self.txt.SetFocus()
        self.txt.Bind(wx.EVT_TEXT_ENTER, self.OnEnter)
        my_sizer.Add(self.txt, 0, wx.ALL, 5)
        panel.SetSizer(my_sizer)
        self.Show()
        os.system(" start Welcome.mp3")

    def OnEnter(self, event):

        input = self.txt.GetValue()
        input = input.lower()
        # noinspection PyBroadException
        try:

            res = client.query(input)
            answer = next(res.results).text
            print (answer)
            os.system("start Answer.mp3")
            text3= answer
            myobj3=gTTS(text=text3,lang=language,slow=False)
            myobj3.save("Response.mp3")
            os.system("start Response.mp3" )
        except:
            # noinspection PyBroadException
               try:
                   input = input.split(' ')
                   input = ' '.join(input[2:])
                   print (wikipedia.summary(input))
                   os.system ( "start Answer.mp3" )
               except:
                   print("I don't Know")


if __name__ == "__main__":
    app = wx.App(True)
    frame = MyFrame()
    app.MainLoop()
