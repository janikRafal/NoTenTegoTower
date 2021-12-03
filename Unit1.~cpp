//---------------------------------------------------------------------------

#include <vcl.h>
#pragma hdrstop

#include "Unit1.h"
#include <IniFiles.hpp>
//---------------------------------------------------------------------------
#pragma package(smart_init)
#pragma link "trayicon"
#pragma resource "*.dfm"
TForm1 *Form1;
//---------------------------------------------------------------------------
AnsiString TForm1::showTime(long long int *time)
{
        AnsiString h, m, s;
        int hours, minutes, seconds;

        hours = *time/3600;
        minutes = (*time - hours*3600)/60;
        seconds = *time - (hours*3600 + minutes*60);

        if (hours < 10) h = "0" + IntToStr(hours);
        else h = IntToStr(hours);
        if (minutes < 10) m = "0" + IntToStr(minutes);
        else m = IntToStr(minutes);
        if (seconds < 10) s = "0" + IntToStr(seconds);
        else s = IntToStr(seconds);

        return h + ":" + m + ":" + s;
}
//---------------------------------------------------------------------------
void TForm1::autoSave()
{
        TIniFile *ini = new TIniFile(ChangeFileExt(Application->ExeName, ".ini"));
        ini -> WriteInteger("Total Time", "startTime", startTime);
        ini -> WriteInteger("Current Time", "currentTime", currentTime);
        if (dateToday.FormatString("dd.mm.yy") == startDate && startTime <= 60)// reset button
        {
                Label1 -> Caption = "Od " + startDate + ":";
                ini -> WriteString("Total Time", "startDate", startDate);
        }
        if (dateToday.FormatString("dd.mm.yy") == currentDate && currentTime <= 60)// reset button
                ini -> WriteString("Current Time", "currentDate", currentDate);

        delete ini;
        counter = 0;
}
//---------------------------------------------------------------------------
__fastcall TForm1::TForm1(TComponent* Owner)
        : TForm(Owner)
{
        dateToday = TDateTime::CurrentDate();

        TIniFile *ini = new TIniFile(ChangeFileExt(Application->ExeName, ".ini"));
        if (FileExists("Timer_v2.ini") == false)
        {
                ini -> WriteString("Total Time", "startDate", dateToday.FormatString("dd.mm.yy"));
                ini -> WriteString("Current Time", "currentDate", dateToday.FormatString("dd.mm.yy"));
        }
        startDate = ini -> ReadString("Total Time", "startDate", dateToday.FormatString("dd.mm.yy"));
        startTime = ini -> ReadInteger("Total Time", "startTime", 0);
        currentDate = ini -> ReadString("Current Time", "currentDate", dateToday.FormatString("dd.mm.yy"));
        if (dateToday.FormatString("dd.mm.yy") != currentDate)
        {
                currentTime = 0;
                ini -> WriteString("Current Time", "currentDate", dateToday.FormatString("dd.mm.yy"));
        }
        else currentTime = ini -> ReadInteger("Current Time", "currentTime", 0);
        delete ini;

        counter = 0;
        Label1 -> Caption = "Od " + startDate + ":";

}
//---------------------------------------------------------------------------

void __fastcall TForm1::Timer1Timer(TObject *Sender)
{
        startTime++;
        currentTime++;

        Label2 -> Caption = showTime(&startTime);
        Label4 -> Caption = showTime(&currentTime);

        counter++;
        if (counter == 60)      //autosave 1 min;
                autoSave();
}
//---------------------------------------------------------------------------

void __fastcall TForm1::Button1Click(TObject *Sender)
{
        Label2 -> Caption = "--:--:--";
        startTime = 0;
        startDate = dateToday.FormatString("dd.mm.yy");
        autoSave();
}
//---------------------------------------------------------------------------

void __fastcall TForm1::Button2Click(TObject *Sender)
{
        Label4 -> Caption = "--:--:--";
        currentTime = 0;
        currentDate = dateToday.FormatString("dd.mm.yy");
        autoSave();
}
//---------------------------------------------------------------------------
void __fastcall TForm1::TrayIcon1Click(TObject *Sender)
{
        Show();
        Application -> BringToFront();
}
//---------------------------------------------------------------------------

void __fastcall TForm1::FormClose(TObject *Sender, TCloseAction &Action)
{
        autoSave();
}
//---------------------------------------------------------------------------

