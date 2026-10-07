# JEPX_spot-curl
JEPX
let
    Url = "https://www.jepx.jp/_download.php",

    PostData = Text.ToBinary(
        "dir=spot_summary&file=spot_summary_2026.csv"
    ),

    Response = Web.Contents(
        Url,
        [
            Headers = [
                #"Content-Type" = "application/x-www-form-urlencoded",
                Referer = "https://www.jepx.jp/electricpower/market-data/spot/"
            ],
            Content = PostData
        ]
    ),

    CsvData = Csv.Document(
        Response,
        [
            Encoding = 932,
            QuoteStyle = QuoteStyle.Csv
        ]
    ),
    昇格されたヘッダー数 = Table.PromoteHeaders(CsvData, [PromoteAllScalars=true]),
    変更された型 = Table.TransformColumnTypes(昇格されたヘッダー数,{{"受渡日", type date}, {"時刻コード", Int64.Type}, {"売り入札量(kWh)", Int64.Type}, {"買い入札量(kWh)", Int64.Type}, {"約定総量(kWh)", Int64.Type}, {"システムプライス(円/kWh)", type number}, {"エリアプライス北海道(円/kWh)", type number}, {"エリアプライス東北(円/kWh)", type number}, {"エリアプライス東京(円/kWh)", type number}, {"エリアプライス中部(円/kWh)", type number}, {"エリアプライス北陸(円/kWh)", type number}, {"エリアプライス関西(円/kWh)", type number}, {"エリアプライス中国(円/kWh)", type number}, {"エリアプライス四国(円/kWh)", type number}, {"エリアプライス九州(円/kWh)", type number}, {"売りブロック入札総量(kWh)", Int64.Type}, {"売りブロック約定総量(kWh)", Int64.Type}, {"買いブロック入札総量(kWh)", Int64.Type}, {"買いブロック約定総量(kWh)", Int64.Type}})
in
    変更された型
