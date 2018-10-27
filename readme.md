full stack data-science finance (small) project
================
Olivier Bauthéac

# preprocessing (ELT)

## extract

### minimum required

In an excel woorkbook, query Bloomberg for historical (bdh) as well as
contemporaneous (bdp) data for a market index as well as a broad
cross-section of U.S. stocks. Historical data should be retrieved from
October 1<sup>st</sup> 2016 to today at the daily frequency on
individual ticker specific sheets (one sheet per name). All names’
contemporaneous data, on the other hand, should sit on a single sheet.
The Bloomberg ticker for the market index is ‘RAY Index’ while those for
the corporation names are listed
below:

| BBG stock tickers |                |                |                |                 |
| ----------------- | -------------- | -------------- | -------------- | --------------- |
| ADM US Equity     | CIVI US Equity | GBX US Equity  | LIND US Equity | SERV US Equity  |
| AE US Equity      | CLGX US Equity | GDI US Equity  | LZB US Equity  | SGA US Equity   |
| AGCO US Equity    | CLR US Equity  | GHC US Equity  | MAN US Equity  | SITE US Equity  |
| AJRD US Equity    | COMM US Equity | GME US Equity  | MEI US Equity  | SMP US Equity   |
| ALG US Equity     | CRL US Equity  | GOLF US Equity | MLR US Equity  | SPXC US Equity  |
| AMD US Equity     | CTB US Equity  | GPN US Equity  | MRC US Equity  | STRT US Equity  |
| AMOT US Equity    | CTLT US Equity | GTLS US Equity | MTD US Equity  | SUPN US Equity  |
| ASGN US Equity    | CTXS US Equity | HFC US Equity  | MTZ US Equity  | TAST US Equity  |
| ATRO US Equity    | DHI US Equity  | HOFT US Equity | NC US Equity   | TMO US Equity   |
| AVT US Equity     | DKS US Equity  | HPE US Equity  | NGVT US Equity | TNET US Equity  |
| AWI US Equity     | EBIX US Equity | HURC US Equity | NHC US Equity  | TPB US Equity   |
| BBBY US Equity    | EEFT US Equity | HWKN US Equity | NUE US Equity  | UBNT US Equity  |
| BFAM US Equity    | ELF US Equity  | HY US Equity   | OSIS US Equity | UFPI US Equity  |
| BID US Equity     | ELVT US Equity | IAC US Equity  | OSK US Equity  | UFS US Equity   |
| BIG US Equity     | EML US Equity  | IART US Equity | PFGC US Equity | USAK US Equity  |
| BKNG US Equity    | ENTG US Equity | IBP US Equity  | PGTI US Equity | VLGEA US Equity |
| BLD US Equity     | ERI US Equity  | IDTI US Equity | PKI US Equity  | VLO US Equity   |
| BSET US Equity    | ETH US Equity  | INT US Equity  | PLPC US Equity | VRSK US Equity  |
| BWA US Equity     | FICO US Equity | IOSP US Equity | PRAH US Equity | WBC US Equity   |
| BYD US Equity     | FISV US Equity | ITRI US Equity | PSX US Equity  | WERN US Equity  |
| CAL US Equity     | FL US Equity   | JLL US Equity  | RBC US Equity  | WGO US Equity   |
| CBRE US Equity    | FLR US Equity  | KHC US Equity  | RS US Equity   | WRK US Equity   |
| CENTA US Equity   | FLT US Equity  | KSU US Equity  | RXN US Equity  | XPO US Equity   |
| CHEF US Equity    | FTV US Equity  | LGND US Equity | SCL US Equity  | ZBRA US Equity  |

The historical time series should include the following market & book
data fields:

| field                | Bloomberg symbol             |
| -------------------- | ---------------------------- |
| close price          | PX\_LAST                     |
| book value per share | BOOK\_VAL\_PER\_SH           |
| earnings per share   | TRAIL\_12M\_EPS              |
| dividend per share   | TRAIL\_12M\_DVD\_PER\_SH     |
| debt                 | SHORT\_AND\_LONG\_TERM\_DEBT |
| equity               | TOTAL\_EQUITY                |
| current assets       | BS\_CUR\_ASSET\_REPORT       |
| current liabilities  | BS\_CUR\_LIAB                |
| sales                | SALES\_REV\_TURN             |

Contemporaneous data on the other hand should include the number of
shares outstanding, number of directors on the board, number of women on
the board, number of board meetings per year, long company name and
companie description. Explore Bloomberg to find the corresponding field
symbols.

### going further

  - Using VBA, make your workbook updatable. Ammend your workbook so
    that it retrieves up to date data in one clic.
      - Hint 1. Update doesn’t necessarily mean adding most recent
        values to an existing time series. Requerying the whole data up
        to the most recent date would work as well.
      - Hint 2. Inspect the BQL syntax in Bloomberg formula cells,
        ammend accordingly.
  - Using VBA, make your workbook flexible. Ammend your workbook so that
    it can retrieve data for any set of stocks/indexes & market/book
    fields at various frequencies (year, month, week, day), from and to
    any date.
      - Hint 1. Object oriented programming could help; excel table
        objects in particular.
      - Hint 2. Create an update sheet with tickers list, parameters
        (frequency, start and end dates) and fields. This sheet could
        also be use to host the contemporaneous dataset.
  - Using VBA, make your workbook fully portable. If you open your
    workbook without a live Bloomberg connection you’ll notice you loose
    the contemporaneous dataset; try to fix that problem somehow.
      - Hint 1. VBA events could help.

You now have a fully customizable & portable tool to retrieve financial
data from Bloomberg and now it’s time to use it.

## Load

In R or Python (examplesolutions will be provided for both programming
languages), load the workbook data in memory. Organise the data in two
dataframes, one for the historical times series, the other for static
(contemporaneous) data. The time series dataframe should have a
two-level row index including tickers & dates while columns should host
the corresponding time series; the dataframe structure should look as
follow:

    ## # A tibble: 62,696 x 11
    ##    ticker    Date       PX_LAST BOOK_VAL_PER_SH TRAIL_12M_EPS
    ##    <chr>     <date>       <dbl>           <dbl>         <dbl>
    ##  1 RAY Index 2016-10-04   1274.            476.          58.4
    ##  2 RAY Index 2016-10-05   1280.            476.          58.4
    ##  3 RAY Index 2016-10-06   1279.            476.          58.4
    ##  4 RAY Index 2016-10-07   1275.            476.          58.5
    ##  5 RAY Index 2016-10-10   1281.            476.          58.5
    ##  6 RAY Index 2016-10-11   1265.            476.          58.5
    ##  7 RAY Index 2016-10-12   1266.            476.          58.5
    ##  8 RAY Index 2016-10-13   1261.            476.          58.5
    ##  9 RAY Index 2016-10-14   1261.            476.          58.5
    ## 10 RAY Index 2016-10-17   1258.            476.          58.6
    ## # ... with 62,686 more rows, and 6 more variables:
    ## #   TRAIL_12M_DVD_PER_SH <dbl>, SHORT_AND_LONG_TERM_DEBT <dbl>,
    ## #   TOTAL_EQUITY <dbl>, BS_CUR_ASSET_REPORT <dbl>, BS_CUR_LIAB <dbl>,
    ## #   SALES_REV_TURN <dbl>

The static dataset on the other hand should be row-indexed by tickers
<<<<<<< HEAD
and have columns hosting the corresponding static fields.
=======
and have columns hosting the corresponding static fields; the dataframe
structure should look as follow:

    ## # A tibble: 120 x 5
    ##    ticker  EQY_SH_OUT NUMBER_OF_DIRECTO~ NUMBER_OF_WOMEN~ BOARD_MEETINGS_~
    ##    <chr>        <dbl>              <dbl>            <dbl>            <dbl>
    ##  1 KHC US~      1219.                 11                2                5
    ##  2 PSX US~       464.                  9                3                6
    ##  3 VLO US~       427.                 10                4                7
    ##  4 ADM US~       560.                 12                2               10
    ##  5 HPE US~      1472.                 12                5               13
    ##  6 NUE US~       316.                  8                2                4
    ##  7 DHI US~       377.                  5                1                7
    ##  8 HFC US~       176.                 11                2               15
    ##  9 KSU US~       102.                 11                3                5
    ## 10 WRK US~       255.                 13                2                8
    ## # ... with 110 more rows
>>>>>>> 5eecc7b5e711901e192970d8e28a7ef71635f091

## Transform
