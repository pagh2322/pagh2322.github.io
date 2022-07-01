---
title: Coordinate - UIViewRepresentable
description: SwiftUI에서 delegate를 채택하자.(UIViewRepresentable)
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- UIKit
- Coordinate
- Delegate
---

# 기존에 보면 좋은 내용
[UIViewRepresentable - SwiftUI에서 UIKit을 사용](https://pagh2322.github.io/swiftui/2022/05/18/swiftui-uiviewrepresentable/)

# Delegate
UIKit에서는 많은 예제 코드들이 delegate을 사용한다. 

필자가 애드몹을 UIKit로 만든 앱에 넣었을 때와 SwiftUI로 만든 앱에 넣었을 때를 비교할 때, delegate를 주로 사용하는 UIKit에서 훨씬 간편했다.

하지만 언젠간 SwiftUI로 앱을 개발할 것이고, 그러기 위해선 SwiftUI에서 제공하는 기능들로 구현을 해야한다. 그래서 위에서 말한 애드몹을 SwiftUI로 만든 앱에 넣었을 때를 회고하며 작성하려고 한다.

# Coordinate
UIKit에서 사용하는 `Coordinate 패턴`과는 다른 개념이다.(물론 Coordinate 패턴을 아직은 모른다..)

아래는 SwiftUI에서 `FSCalendar`를 구현하려는 코드이다.

```swift
struct CalendarView: UIViewRepresentable {
    typealias UIViewType = FSCalendar
    
    var scope: FSCalendarScope
    @Binding var date: Date
    @Binding var toolbarTitle: String
    @ObservedObject var abViewModel: AccountBookViewModel
    
    func makeUIView(context: Context) -> FSCalendar {
        let calendar = FSCalendar()
        calendar.delegate = context.coordinator
        calendar.dataSource = context.coordinator
        calendar.scope = self.scope
        
        calendar.locale = Locale(identifier: "ko_KR")
        calendar.appearance.headerDateFormat = "YYYY년 MM월"
        calendar.appearance.headerMinimumDissolvedAlpha = 0.0
        calendar.appearance.headerTitleColor = UIColor(.fontColor)
        calendar.appearance.weekdayTextColor = UIColor(.weekdayColor)
        
        calendar.appearance.titleDefaultColor = UIColor(.calendarText)  // 평일
        calendar.appearance.titleWeekendColor = UIColor(.calendarText)    // 주말
        calendar.appearance.selectionColor = UIColor(.whip)
        calendar.appearance.subtitlePlaceholderColor = UIColor(.subtitlePlaceholder)
        
        calendar.appearance.titleFont = .boldSystemFont(ofSize: 13)
        calendar.appearance.weekdayFont = .boldSystemFont(ofSize: 15)
        calendar.appearance.headerTitleFont = .boldSystemFont(ofSize: 17)
        
        if self.scope == .week {
            calendar.headerHeight = 28
            calendar.appearance.headerTitleFont = .boldSystemFont(ofSize: 0)
            calendar.appearance.titleFont = .boldSystemFont(ofSize: 17)
        } else {
            calendar.headerHeight = UIScreen.main.bounds.height / 11.0
        }
        calendar.select(Date())
        
        let currentPageDate = self.date
        let month = Calendar.current.component(.month, from: currentPageDate)
        let week = Calendar.current.component(.weekOfMonth, from: currentPageDate)
        
        switch self.scope {
        case .week:
            self.toolbarTitle = "\(month)월 \(week)주"
        case .month:
            self.toolbarTitle = "\(month)월"
        default :
            self.toolbarTitle = ""
        }
        
        return calendar
    }
    
    func updateUIView(_ uiView: FSCalendar, context: Context) {
        uiView.appearance.todayColor = .white
        uiView.appearance.titleTodayColor = UIColor(.calendarText)
    }
    
    class Coordinator: 
        NSObject, 
        FSCalendarDelegate, 
        FSCalendarDataSource 
    {
        private let calendarView: CalendarView
        
        init(_ calendarView: CalendarView) {
            self.calendarView = calendarView
        }
        
        // 날짜 선택 시 콜백 메소드
        func calendar(
            _ calendar: FSCalendar, 
            didSelect date: Date, 
            at monthPosition: FSCalendarMonthPosition) 
        {
            self.calendarView.date = date
            self.calendarView.abViewModel.currentDate = date
            self.calendarView.abViewModel.change()
        }
        
        func calendarCurrentPageDidChange(_ calendar: FSCalendar) {
            let currentPageDate = calendar.currentPage
            let month = Calendar.current.component(.month, from: currentPageDate)
            let week = Calendar.current.component(.weekOfMonth, from: currentPageDate)
            
            switch calendar.scope {
            case .week:
                self.calendarView.toolbarTitle = "\(month)월 \(week)주"
            case .month:
                self.calendarView.toolbarTitle = "\(month)월"
            }
        }
    }
    
    func makeCoordinator() -> Coordinator {
        return Coordinator(self)
    }
}

```

나눠서 보자..

## class Coordinator
CalendarView 구조체 안에 Coordinator라는 클래스를 추가하였다. 채택할 delegate의 이름을 적어주는 것도 잊지말자.

그리고 Coordinator는 **context**에 의해 아래 **makeCoordinator** 메소드를 통해 생성된다.

아래 코드에선 FSCalendar의 delegate 메소드들을 적어주었다.

```swift
class Coordinator: 
    NSObject, 
    FSCalendarDelegate, 
    FSCalendarDataSource 
{
    private let calendarView: CalendarView
        
    init(_ calendarView: CalendarView) {
        self.calendarView = calendarView
    }
        
        // 날짜 선택 시 콜백 메소드
    func calendar(
        _ calendar: FSCalendar, 
        didSelect date: Date, 
        at monthPosition: FSCalendarMonthPosition
    ) { 
        // ...
    }
        
    func calendarCurrentPageDidChange(_ calendar: FSCalendar) {
        // ...
    }
}
```

## makeCoordinator
makeCoordinator를 구현하여, CalendarView 구조체가 Coordinator의 존재를 알게 해준다. 해당 함수는 makeUIView에서 할 필요 없이 자동으로 호출이 된다.

```swift
func makeCoordinator() -> Coordinator {
    return Coordinator(self)
}
```

## 임위자 선택

```swift
func makeUIView(context: Context) -> FSCalendar {
    let calendar = FSCalendar()
    calendar.delegate = context.coordinator
    calendar.dataSource = context.coordinator
    // ...
}
```

# 참고 링크
[[iOS] SwiftUI에서는 delegate를 어떻게 채택할까? : Coordinator](https://sweetdev.tistory.com/447)