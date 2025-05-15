# Component

大方向, 頁面級別交換Token另開視窗，組件使用模組化

## 角色說明
- A: IGroup
- B: 配合廠商

## 元件規格
- `B`提供需支援 `ReactJS` v18.0 - v19.1
- `B`提供需為 `React Component`/`Provider`
- `B`提供需支援 `Typescript` 或 `.d.ts` 安全型別
- `B`需要提供 `Storybook` 作為開發者文件 與`A`確認互動效果
- `B`需要上傳到 `NPM` 提供給`A`前端單位 進行 npm install, import 使用 (不需要提供 `SourceCode` 給`A`前端, 由`B`自行管理與維護)

## 規則說明
- Step1. 觸發進入紅包雨畫面由前端控制 (`A`前端查詢`A`後端API取得是否符合資格條件的結果決定)。

如果條件成立，`A`前端渲染`B`提供的Module元件，並且進行渲染元件到畫面中
 > 需提供自動開始的 Props: autoPlay={true}
 > 手動控制 `B` 提供 Ref 綁定讓前端可以透過 Ref 呼叫 play()
 
- Step2. 開始播放紅包雨之後的邏輯畫面由 `B` 規劃與執行
> `B`規劃與執行 包含最多紅包抽幾個、抽到什麼、紅包下降多久的遊戲時間
- Step3. 當遊戲結束時，`B`元件需提供 props onEnd 事件讓`A`可處理關閉
> `B`通知`A`的遊戲結果由兩邊的後端溝通

## 額外說明
- 如果規劃中 `B` 包含得獎畫面，則透過 `A`與`B`交換Token的方式 開啟新視窗 (類似啟動遊戲館)

## 使用方式需求

```tsx
import {RedEnvelopeRain} from '@kc/red-Envelope-rain'

/**
 * Sample Layout
 */
const ExamplePage = () => {

    const handleGoPlay = () => {
        GameModal.show();
    }
    
    return <div>
        {/* 頁面中的內容 */}
        
        <GamePlayButton onClick={handleGoPlay}>
            抓紅包囉！
        </GamePlayButton>
    </div>;
}


/**
 * 遊戲載入區塊
 */
const GameModal = () => {
    const callApi = useAPIMuatation();
    const ref = useRef<RedEnvelopeRain>(null);

    useEffect(() => {
        ref.current.play();
    }, []);
    
    const handleOnEnd = useCallback(() => {
        // A前端自己呼叫關閉光箱
    }, [])
    
    return <>
        <RedEnvelopeRain 
            ref={ref}
            autoPlay={false}
            onEnd={handleOnHide}
        />; 
    </>;
}
```


