    %let pgm=utl-how-to-make-lines-between-stack-bar-charts-plot-graph;

    How to make lines between stack bar charts? plot graph

    github
    https://tinyurl.com/335umz4d
    https://github.com/rogerjdeangelis/utl-how-to-make-lines-between-stack-bar-charts-plot-graph

    stackoverflow
    https://tinyurl.com/yckh8wz2
    https://stackoverflow.com/questions/79374833/how-to-make-lines-between-stack-bar-charts

    Solution by michiel-duvekot
    https://stackoverflow.com/users/4665356/michiel-duvekot

    hi res graph output
    https://tinyurl.com/kjz79ydb
    https://github.com/rogerjdeangelis/utl-how-to-make-lines-between-stack-bar-charts-plot-graph/blob/main/bars.png

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                             |                                  |                                                       */
    /*        INPUT                |      PROCESS                     |                      OUTPUT                           */
    /*        =====                |      =======                     |                      ======                           */
    /*                             |                                  |                                                       */
    /*options validvarname=upcase; | %utlfkil(d:/png/bars.png)        |                                                       */
    /* libname sd1 "d:/sd1";       |                                  |       |--- NONESPON --|    |--- RESPONDE --|  GROUP   */
    /* data sd1.have;              | options ls=96;                   |         POST     PREV       POST        PREV  TIME    */
    /*   input Group$ Time$        | %utl_rbeginx;                    |     ---------------------------------------------     */
    /*       CellType$ Proportion; | parmcards4;                      |     |                                           |     */
    /* cards4;                     | library(haven)                   |   .4+            WHITE      WHITE-------WHITE   +.4   */
    /* RESPONDE PREV HEMOC 0.0967  | library(sqldf)                   | C   |           /WHITE      WHITE       WHITE   |   C */
    /* RESPONDE POST HEMOC 0.1441  | library(tidyverse)               | E   |          / WHITE      WHITE       WHITE   |   E */
    /* RESPONDE PREV MONOC 0.0752  | data<-read_sas(                  | L   |         /  WHITE      WHITE       WHITE   |   L */
    /* RESPONDE POST MONOC 0.0234  |  "d:/sd1/have.sas7bdat")         | L   |   WHITE/   WHITE      WHITE     / PLATE   |   L */
    /* RESPONDE PREV LIMBI 0.0860  | data                             | P .3+   WHITE    WHITE      WHITE    // PLATE   +.3 P */
    /* RESPONDE POST LIMBI 0.0905  | png("d:/png/bars.png")           | R   |   WHITE----WHITE      WHITE   //  PLATE   |   R */
    /* RESPONDE PREV CD4_T 0.1053  | data %>%                         | O   |   PLATE----PLATE      WHITE  //   PLATE   |   O */
    /* RESPONDE POST CD4_T 0.0960  |  group_by(GROUP, TIME) %>%       | P   |   PLATE    PLATE      WHITE //    PLATE   |   P */
    /* RESPONDE PREV CD8_T 0.2602  |  mutate(                         | O   |   PLATE    PLATE      WHITE//     PLATE   |   O */
    /* RESPONDE POST CD8_T 0.1910  |    PROPORTION= PROPORTION,       | R .2+   PLATE\   PLATE      PLATE/      PLATE   +.2 R */
    /* RESPONDE PREV WHITE 0.0817  |    TIME= factor(TIME)            | T   |   MONOC\\  PLATE      PLATE       PLATE   |   T */
    /* RESPONDE POST WHITE 0.2011  |    ) %>%                         | I   |   MONOC \\ PLATE      PLATE      /PLATE   |   I */
    /* RESPONDE PREV OTHER 0.0602  |  ungroup() %>%                   | O   |   MONOC  \\PLATE      PLATE     / MONOC   |   O */
    /* RESPONDE POST OTHER 0.0770  |  ggplot(                         | N   |   MONOC   \PLATE      PLATE    / /MONOC   |   N */
    /* RESPONDE PREV PLATE 0.2344  |    aes(x=TIME,y=PROPORTION       | S .1+   MONOC    MONOC      PLATE   / / MONOC   +.1 S */
    /* RESPONDE POST PLATE 0.1765  |      ,fill=CELLTYPE))+           |     |   MONOC    MONOC      PLATE  / /  MONOC   |     */
    /* NONESPON PREV HEMOC 0.1290  |  geom_bar(stat="identity"        |     |   MONOC    MONOC      PLATE / /   MONOC   |     */
    /* NONESPON POST HEMOC 0.1237  |   ,position="stack",width=.5) +  |     |   MONOC    MONOC      PLATE/ /    MONOC   |     */
    /* NONESPON PREV MONOC 0.1028  |  facet_wrap(~ GROUP, ncol= 2) +  |     |   MONOC----MONOC      MONOC/------MONOC   |     */
    /* NONESPON POST MONOC 0.1840  |  scale_fill_brewer(              |     ---------------------------------------------     */
    /* NONESPON PREV LIMBI 0.0626  |       palette="Paired") +        |    TIME POST     PREV       POST        PREV  TIME    */
    /* NONESPON POST LIMBI 0.1247  |  theme_minimal() +               |         |- NONESPON-|      |-----RESPONDE --| GROUP   */
    /* NONESPON PREV CD4_T 0.1365  |  labs(title= "Cell Proportions", |                                                       */
    /* NONESPON POST CD4_T 0.2034  |    x = "Time",                   |                          CELLTYPES                    */
    /* NONESPON PREV CD8_T 0.1777  |    y = "Cell Proportions",       |         MONOC              PLATE              WHITE   */
    /* NONESPON POST CD8_T 0.0552  |    fill = "Cell Type"            |                                                       */
    /* NONESPON PREV WHITE 0.1412  |    ) +                           |                                                       */
    /* NONESPON POST WHITE 0.0552  |  geom_line(                      |                                                       */
    /* NONESPON PREV OTHER 0.0869  |   aes(                           |                                                       */
    /* NONESPON POST OTHER 0.1820  |     x=ifelse(TIME=="POST"        |                                                       */
    /* NONESPON PREV PLATE 0.1627  |       ,as.numeric(TIME)+0.25     |                                                       */
    /* NONESPON POST PLATE 0.0715  |       ,as.numeric(TIME)-.25),    |                                                       */
    /* ;;;;                        |     y = PROPORTION,              |                                                       */
    /* run;quit;                   |     group = CELLTYPE),           |                                                       */
    /*                             |  position = position_stack(),    |                                                       */
    /*                             |  linewidth=1.5,                  |                                                       */
    /*                             |  linetype = "solid")             |                                                       */
    /*                             | dev.off()                        |                                                       */
    /*                             | ;;;;                             |                                                       */
    /*                             | %utl_rendx;                      |                                                       */
    /*                             |                                  |                                                       */
    /*                             |                                  |                                                       */
    /*******************************|******************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have ;
      input Group$ Time$
          CellType$ Proportion;
    cards4;
    RESPONDE PREV HEMOC 0.0967
    RESPONDE POST HEMOC 0.1441
    RESPONDE PREV MONOC 0.0752
    RESPONDE POST MONOC 0.0234
    RESPONDE PREV LIMBI 0.0860
    RESPONDE POST LIMBI 0.0905
    RESPONDE PREV CD4_T 0.1053
    RESPONDE POST CD4_T 0.0960
    RESPONDE PREV CD8_T 0.2602
    RESPONDE POST CD8_T 0.1910
    RESPONDE PREV WHITE 0.0817
    RESPONDE POST WHITE 0.2011
    RESPONDE PREV OTHER 0.0602
    RESPONDE POST OTHER 0.0770
    RESPONDE PREV PLATE 0.2344
    RESPONDE POST PLATE 0.1765
    NONESPON PREV HEMOC 0.1290
    NONESPON POST HEMOC 0.1237
    NONESPON PREV MONOC 0.1028
    NONESPON POST MONOC 0.1840
    NONESPON PREV LIMBI 0.0626
    NONESPON POST LIMBI 0.1247
    NONESPON PREV CD4_T 0.1365
    NONESPON POST CD4_T 0.2034
    NONESPON PREV CD8_T 0.1777
    NONESPON POST CD8_T 0.0552
    NONESPON PREV WHITE 0.1412
    NONESPON POST WHITE 0.0552
    NONESPON PREV OTHER 0.0869
    NONESPON POST OTHER 0.1820
    NONESPON PREV PLATE 0.1627
    NONESPON POST PLATE 0.0715
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*    GROUP      TIME    CELLTYPE    PROPORTION                                                                           */
    /*                                                                                                                        */
    /*   RESPONDE    PREV     HEMOC        0.0967                                                                             */
    /*   RESPONDE    POST     HEMOC        0.1441                                                                             */
    /*   RESPONDE    PREV     MONOC        0.0752                                                                             */
    /*   RESPONDE    POST     MONOC        0.0234                                                                             */
    /*   RESPONDE    PREV     LIMBI        0.0860                                                                             */
    /* ...                                                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utlfkil(d:/png/bars.png)

    options ls=96;
    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    library(tidyverse)
    data<-read_sas(
     "d:/sd1/have.sas7bdat")
    data
    png("d:/png/bars.png")
    data %>%
     group_by(GROUP, TIME) %>%
     mutate(
       PROPORTION= PROPORTION,
       TIME= factor(TIME)
       ) %>%
     ungroup() %>%
     ggplot(
       aes(x=TIME,y=PROPORTION
         ,fill=CELLTYPE))+
     geom_bar(stat="identity"
      ,position="stack",width=.5) +
     facet_wrap(~ GROUP, ncol= 2) +
     scale_fill_brewer(
          palette="Paired") +
     theme_minimal() +
     labs(title= "Cell Proportions",
       x = "Time",
       y = "Cell Proportions",
       fill = "Cell Type"
       ) +
     geom_line(
      aes(
        x=ifelse(TIME=="POST"
          ,as.numeric(TIME)+0.25
          ,as.numeric(TIME)-.25),
        y = PROPORTION,
        group = CELLTYPE),
     position = position_stack(),
     linewidth=1.5,
     linetype = "solid")
    dev.off()
    ;;;;
    %utl_rendx;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */


    proc chart data=sd1.have(where=(celltype in ("MONOC","WHITE","PLATE")));
     vbar time / group=Group subgroup=celltype sumvar=proportion;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SIMUALTION                                                                                                            */
    /*                                                                                                                        */
    /*        |--- NONESPON --|    |--- RESPONDE --|  GROUP                                                                   */
    /*          POST     PREV       POST        PREV  TIME                                                                    */
    /*      ---------------------------------------------                                                                     */
    /*      |                                           |                                                                     */
    /*    .4+            WHITE      WHITE-------WHITE   +.4                                                                   */
    /*  C   |           /WHITE      WHITE       WHITE   |   C                                                                 */
    /*  E   |          / WHITE      WHITE       WHITE   |   E                                                                 */
    /*  L   |         /  WHITE      WHITE       WHITE   |   L                                                                 */
    /*  L   |   WHITE/   WHITE      WHITE     / PLATE   |   L                                                                 */
    /*  P .3+   WHITE    WHITE      WHITE    // PLATE   +.3 P                                                                 */
    /*  R   |   WHITE----WHITE      WHITE   //  PLATE   |   R                                                                 */
    /*  O   |   PLATE----PLATE      WHITE  //   PLATE   |   O                                                                 */
    /*  P   |   PLATE    PLATE      WHITE //    PLATE   |   P                                                                 */
    /*  O   |   PLATE    PLATE      WHITE//     PLATE   |   O                                                                 */
    /*  R .2+   PLATE\   PLATE      PLATE/      PLATE   +.2 R                                                                 */
    /*  T   |   MONOC\\  PLATE      PLATE       PLATE   |   T                                                                 */
    /*  I   |   MONOC \\ PLATE      PLATE      /PLATE   |   I                                                                 */
    /*  O   |   MONOC  \\PLATE      PLATE     / MONOC   |   O                                                                 */
    /*  N   |   MONOC   \PLATE      PLATE    / /MONOC   |   N                                                                 */
    /*  S .1+   MONOC    MONOC      PLATE   / / MONOC   +.1 S                                                                 */
    /*      |   MONOC    MONOC      PLATE  / /  MONOC   |                                                                     */
    /*      |   MONOC    MONOC      PLATE / /   MONOC   |                                                                     */
    /*      |   MONOC    MONOC      PLATE/ /    MONOC   |                                                                     */
    /*      |   MONOC----MONOC      MONOC/------MONOC   |                                                                     */
    /*      ---------------------------------------------                                                                     */
    /*     TIME POST     PREV       POST        PREV  TIME                                                                    */
    /*          |- NONESPON-|      |-----RESPONDE --| GROUP                                                                   */
    /*                                                                                                                        */
    /*                           CELLTYPES                                                                                    */
    /*          MONOC              PLATE              WHITE                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

