# Gravity
Satellite &amp; planet
#include "stdafx.h"
#include <iostream>
#include <stdlib.h>
#include <math.h>
#include <windows.h>
#include "resource.h"
using namespace std;
#define PI 3.14;

using namespace std;
double dt = 0.5;
double  G = 6.67;
int Msp = 10, Mpl = 1000, xpl, ypl, xsp, ysp;
int R = 100;
double v0 = sqrt((G*Mpl / R));
double vx = v0, vy = 0;
double T0 = 4 * 3.14*3.14*pow(R, 3) / G / Mpl;
int WINAPI DlgProc(HWND hDlg, WORD wMsg, WORD wParam, DWORD)
{
	PAINTSTRUCT ps;
	if (wMsg == WM_CLOSE || wMsg == WM_COMMAND && wParam == IDOK)
	{
		EndDialog(hDlg, 0);
	}
	else if (wMsg == WM_INITDIALOG)//узнаем размеры окна
	{
		RECT rc;
		GetClientRect(hDlg, &rc);
		int dx = rc.right - rc.left;
		int dy = rc.bottom - rc.top;
	}

	else if (wMsg == WM_PAINT)
	{
		BeginPaint(hDlg, &ps);//далее создаем перо
		HPEN hPen = (HPEN)CreatePen(PS_SOLID, 7, RGB(0, 250, 0));
		HPEN hOldPen = (HPEN)SelectObject(ps.hdc, hPen);
		POINT ptOld; //обрисовка  расчитанных писксельных координат
					 //SetPixel(ps.hdc, xpl, ypl, RGB(255, 0, 0));//то планета
		for (int i = 0; i < (T0 / dt); i++)
		{
			vx = vx - G * Mpl*xsp*dt / (pow(R, 3));
			vy = vy - G * Mpl*ysp*dt / (pow(R, 3));
			xsp = static_cast<int>(xsp + vx * dt);
			ysp = static_cast<int>(ysp + vy * dt);
			cout << xsp << " " << ysp << endl;
			MoveToEx(ps.hdc, int(xsp), int(ysp), &ptOld);
			LineTo(ps.hdc, int(xsp), int(ysp));
		}
		SelectObject(ps.hdc, hOldPen);//уничтожение пера
		DeleteObject(hPen);
		HPEN hPen1 = (HPEN)CreatePen(PS_SOLID, 7, RGB(250, 0, 0));
		//HPEN hOldPen = (HPEN)SelectObject(ps.hdc, hPen1);
		SetPixel(ps.hdc, xpl, ypl, RGB(255, 0, 0));//то планета
		SelectObject(ps.hdc, hOldPen);//уничтожение пера
		DeleteObject(hPen1);

		EndPaint(hDlg, &ps);
	}
	return 0;
}
int main()
{
	cout << "Enter x0,y0" << endl << flush;
	cin >> xpl >> ypl;
	cout << endl;
	xsp = xpl;
	ysp = xpl - R;
	//cout << "Enter R" << endl << flush;
	//cin >> R;
	v0 = sqrt((G*Mpl / R));
	T0 = 4 * 3.14*3.14*pow(R, 3) / G / Mpl;
	cout << "Speed = " << v0 << " & Period = " << T0 << endl << flush;
	double dt = 1 / 2;
	double vx = v0, vy = 0;
	double tmax = 50;
	for (double t = 0; t <= (3 * T0); t = t + 0.5)
	{
		vx = vx - G * Mpl*xpl*dt / (pow(R, 3));
		vy = vy - G * Mpl*ypl*dt / (pow(R, 3));
		xsp = static_cast<int>(xsp + vx * dt);
		ysp = static_cast<int>(ysp + vy * dt);
	}
	DialogBox(NULL, MAKEINTRESOURCE(IDD_DIALOG1), NULL, (DLGPROC)DlgProc);
	int a;
	cin >> a;
	return 0;
}
