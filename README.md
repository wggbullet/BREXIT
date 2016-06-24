import statsmodels.tsa.stattools as ts
import numpy
import matplotlib.pyplot as plt
import sys
from scipy.stats.stats import pearsonr
from datetime import datetime
from pandas.io.data import *

def getdata(sym, start_date, end_date, savedata=False):
    """ readin data from yahoo, and save it """
    starty = int(str(start_date)[:4])
    startm = int(str(start_date)[4:6])
    startd = int(str(start_date)[6:8])
    
    endy = int(str(end_date)[:4])
    endm = int(str(end_date)[4:6])
    endd = int(str(end_date)[6:8])

#    print "downloading %s from yahoo...." % (sym)
    goog=DataReader(sym,"yahoo", start=datetime(starty, startm, startd), end=datetime(endy, endm, end
d))
    return_list = []

    if len(goog) > 200:
        return_list.append(goog.index)
        return_list.append(goog['Open'])
        return_list.append(goog['High'])
        return_list.append(goog['Low'])
        return_list.append(goog['Adj Close'])
        return_list.append(goog['Volume'])
  #      return_list.append(goog['Adj Close'])
        
        if savedata:
        #    outfile = "data/%s_%s_%s.csv" % (sym, str(start_date), str(end_date))
            outfile = "data/%s.csv" % (sym)
            outF = open(outfile, 'w')
            for i in range(len(return_list[0])):
                ostr = ""
                for j in range(len(return_list)):
                    ostr += str(return_list[j][i]) + ","
                ostr += "\n"
                outF.write(ostr)
            outF.close()
    return return_list


def getdata_from_disk(sym, start_date, end_date):
    infile = "data/%s.csv" % (sym)
    inF = open(infile, 'r')
    lines = inF.readlines()
    inF.close()
    return_list = [[],[],[]]
    for line in lines:
        fld = line.split(",")
        date = fld[0]
        date2 = str(date).replace("-", "")
        cp = float(fld[1])
        op = float(fld[2])
        if int(date2.split()[0]) < int(start_date): continue
        if int(date2.split()[0]) > int(end_date): continue
        dt = datetime.strptime(date, '%Y-%m-%d %H:%M:%S')
        return_list[0].append(dt)
        return_list[1].append(float(cp))
        return_list[2].append(float(op))

    return_list = numpy.array(return_list)        
    return return_list        


def get_tickers(filename):
    """ readin all the stock tickers """
    tickerF = open(filename, "r")
    lines = tickerF.readlines()
    tickerF.close()
    symbols = []
    for line in lines:
        fld = line.split()
        if fld[0][0] == "#": continue   
        symbols.append(fld[0])
    return symbols 
    
    
from utilities import *
import numpy as np
import pandas as pd

#cps.astype(numpy.float)

def read_sp500_names(filename):
        df = pd.read_csv(filename, sep=',', comment='#')
        return df

def scan_prod(stock_list):
        sym = 'FXB'
        start_date = '20150101'
        end_date = '20160101'
        
        sym = 'FXB'
        tmp_list = getdata(sym, start_date, end_date, savedata=False)
        days = tmp_list[0]
        closes = tmp_list[-2].astype(np.float)
        leg1_dic = {days[i]: closes[i] for i in range(len(days))}
#       print leg1_dic

        for stock in stock_list:
                if str(stock) < 1: continue
                if stock is None: continue
                tmp_list = getdata(stock, start_date, end_date, savedata=False)
                days = tmp_list[0]
                closes = tmp_list[-2].astype(np.float)

                prices = [[], []]



sp_df = read_sp500_names('sp500.txt') 
scan_prod(sp_df['Stock'].tolist())



Stock
MMM
ABT
ABBV
ACN
ATVI
AYI
ADBE
AAP
AES
AET
AFL
AMG
A
GAS
APD
AKAM
ALK
AA
AGN
ALXN
ALLE
ADS
ALL
GOOGL
GOOG
MO
AMZN
AEE
AAL
AEP
AXP
AIG
AMT
AWK
AMP
ABC
AME
AMGN
APH
APC
ADI
AON
APA
AIV
AAPL
AMAT
ADM
AJG
AIZ
T
ADSK
ADP
AN
AZO
AVGO
AVB
AVY
BHI
BLL
BAC
BK
BCR
BAX
BBT
BDX
BBBY
BRK-B
BBY
BIIB
BLK
HRB
BA
BWA
BXP
BSX
BMY
BF-B
CHRW
CA
COG
CPB
COF
CAH
HSIC
KMX
CCL
CAT
CBG
CBS
CELG
CNC
CNP
CTL
CERN
CF
SCHW
CHK
CVX
CMG
CB
CHD
CI
XEC
CINF
CTAS
CSCO
C
CTXS
CLX
CME
CMS
COH
KO
CTSH
CL
CPGX
CMCSA
CMA
CAG
CXO
COP
ED
STZ
GLW
COST
CCI
CSRA
CSX
CMI
CVS
DHI
DHR
DRI
DVA
DE
DLPH
DAL
XRAY
DVN
DO
DLR
DFS
DISCA
DISCK
DG
DLTR
D
DOV
DOW
DPS
DTE
DD
DUK
DNB
ETFC
EMN
ETN
EBAY
ECL
EIX
EW
EA
EMC
EMR
ENDP
ETR
EOG
EQT
EFX
EQIX
EQR
ESS
EL
ES
EXC
EXPE
EXPD
ESRX
EXR
XOM
FFIV
FB
FAST
FRT
FDX
FIS
FITB
FSLR
FE
FISV
FLIR
FLS
FLR
FMC
FTI
FL
F
FBHS
BEN
FCX
FTR
GPS
GRMN
GD
GE
GGP
GIS
GM
GPC
GILD
GPN
GS
GT
GWW
HAL
HBI
HOG
HAR
HRS
HIG
HAS
HCA
HCP
HP
HES
HPE
HOLX
HD
HON
HRL
HST
HPQ
HUM
HBAN
ITW
ILMN
IR
INTC
ICE
IBM
IP
IPG
IFF
INTU
ISRG
IVZ
IRM
JEC
JBHT
JNJ
JCI
JPM
JNPR
KSU
K
KEY
KMB
KIM
KMI
KLAC
KSS
KHC
KR
LB
LLL
LH
LRCX
LM
LEG
LEN
LVLT
LUK
LLY
LNC
LLTC
LKQ
LMT
L
LOW
LYB
MTB
MAC
M
MNK
MRO
MPC
MAR
MMC
MLM
MAS
MA
MAT
MKC
MCD
MCK
MJN
WRK
MDT
MRK
MET
KORS
MCHP
MU
MSFT
MHK
TAP
MDLZ
MON
MNST
MCO
MS
MOS
MSI
MUR
MYL
NDAQ
NOV
NAVI
NTAP
NFLX
NWL
NFX
NEM
NWSA
NWS
NEE
NLSN
NKE
NI
NBL
JWN
NSC
NTRS
NOC
NRG
NUE
NVDA
ORLY
OXY
OMC
OKE
ORCL
OI
PCAR
PH
PDCO
PAYX
PYPL
PNR
PBCT
PEP
PKI
PRGO
PFE
PCG
PM
PSX
PNW
PXD
PBI
PNC
RL
PPG
PPL
PX
CFG
PCLN
PFG
PG
PGR
PLD
PRU
PEG
PSA
PHM
PVH
QRVO
PWR
QCOM
DGX
RRC
RTN
O
RHT
REGN
RF
RSG
RAI
RHI
ROK
COL
ROP
ROST
RCL
R
CRM
SCG
SLB
SNI
STX
SEE
SRE
SHW
SIG
SPG
SWKS
SLG
SJM
SNA
SO
LUV
SWN
SE
SPGI
STJ
SWK
SPLS
SBUX
HOT
STT
SRCL
SYK
STI
SYMC
SYF
SYY
TROW
TGT
TEL
TE
TGNA
TDC
TSO
TXN
TXT
HSY
TRV
TMO
TIF
TWX
TJX
TMK
TSS
TSCO
TDG
RIG
TRIP
FOXA
FOX
TSN
TYC
UDR
ULTA
USB
UA
UNP
UAL
UNH
UPS
URI
UTX
UHS
UNM
URBN
VFC
VLO
VAR
VTR
VRSN
VRSK
VZ
VRTX
VIAB
V
VNO
VMC
WMT
WBA
DIS
WM
WAT
ANTM
WFC
HCN
WDC
WU
WY
WHR
WFM
WMB
WLTW
WEC
WYN
WYNN
XEL
XRX
XLNX
XL
XYL
YHOO
YUM
ZBH
ZION
ZTS
