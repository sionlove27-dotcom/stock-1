import streamlit as st
import yfinance as yf
import pandas as pd
import plotly.graph_objects as go

st.set_page_config(
    page_title="글로벌 빅테크 주가 분석기",
    page_icon="📈",
    layout="wide"
)

st.title("📈 최근 1년 주가 변동 분석")
st.write("삼성전자, SK하이닉스, 구글, 마이크로소프트, 애플 비교")

stocks = {
    "삼성전자": "005930.KS",
    "SK하이닉스": "000660.KS",
    "구글": "GOOGL",
    "마이크로소프트": "MSFT",
    "애플": "AAPL"
}

@st.cache_data
def load_data():
    data = pd.DataFrame()

    for name, ticker in stocks.items():
        df = yf.download(
            ticker,
            period="1y",
            auto_adjust=True,
            progress=False
        )

        data[name] = df["Close"]

    return data

data = load_data()

# 수익률 계산
returns = data / data.iloc[0] * 100

st.subheader("📊 최근 1년 수익률 비교")

fig = go.Figure()

for col in returns.columns:
    fig.add_trace(
        go.Scatter(
            x=returns.index,
            y=returns[col],
            mode="lines",
            name=col
        )
    )

fig.update_layout(
    height=650,
    hovermode="x unified",
    xaxis_title="날짜",
    yaxis_title="수익률 (기준=100)",
    legend_title="종목",
)

st.plotly_chart(fig, use_container_width=True)

st.subheader("📌 현재 성과")

result = pd.DataFrame({
    "종목": returns.columns,
    "1년 수익률(%)": [
        round((returns[col].iloc[-1] - 100), 2)
        for col in returns.columns
    ]
})

result = result.sort_values(
    "1년 수익률(%)",
    ascending=False
)

st.dataframe(
    result,
    use_container_width=True
)

winner = result.iloc[0]

st.success(
    f"최근 1년 최고 수익률 종목: {winner['종목']} ({winner['1년 수익률(%)']}%)"
)

with st.expander("분석 방법"):
    st.write("""
    • 최근 1년 종가 데이터 사용
    • 시작일 가격을 100으로 정규화
    • 배당 및 액면분할 반영 가격 사용
    • Plotly 인터랙티브 차트 제공
    """)
