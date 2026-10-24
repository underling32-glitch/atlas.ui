--[[
    ATLAS UI — clean, muted mobile-first interface demo
    UI library: Kyotaah/lucidui (MIT)
    Intended for Delta on Android, but runtime compatibility is not guaranteed.
    This is an interface template only; controls are demonstrative and do not alter gameplay.
]]

local LIBRARY_URL = "https://raw.githubusercontent.com/Kyotaah/lucidui/main/dist/lucidui.lua"

local requestOk, source = pcall(function()
    return game:HttpGet(LIBRARY_URL)
end)

if not requestOk or type(source) ~= "string" or #source < 1000 then
    warn("ATLAS UI: could not download LucidUI")
    return
end

if type(loadstring) ~= "function" then
    warn("ATLAS UI: loadstring is not available in this environment")
    return
end

local compileOk, loader = pcall(loadstring, source)
if not compileOk or type(loader) ~= "function" then
    warn("ATLAS UI: LucidUI could not be compiled: " .. tostring(loader))
    return
end

local libraryOk, LucidUI = pcall(loader)
if not libraryOk or type(LucidUI) ~= "table" then
    warn("ATLAS UI: LucidUI could not be initialized")
    return
end

local Window = LucidUI:CreateWindow({
    Name = "ATLAS",
    Theme = "Nord",
    Width = 540,
    Height = 410,
    IntroEnabled = false, -- skip splash particles and animated intro
    PillKeybind = Enum.KeyCode.Home,
})

Window:BuildSettingsPanel()

local notificationsEnabled = true
local notificationDuration = 3

-- OVERVIEW
local OverviewTab = Window:CreateTab({ Name = "Обзор", Icon = "home" })
local Overview = OverviewTab:CreateSection({ Name = "Главная" })

Overview:CreateTextDisplay({
    Title = "ATLAS  /  INTERFACE",
    Content = "Спокойная тёмная тема, компактные карточки и управление касанием. Настрой внешний вид через меню настроек в заголовке.",
})

Overview:CreateButton({
    Name = "Проверить уведомление",
    Callback = function()
        if notificationsEnabled then
            LucidUI:Notify({
                Title = "ATLAS",
                Message = "Интерфейс работает.",
                Duration = notificationDuration,
            })
        end
    end,
})

Overview:CreateToggle({
    Name = "Уведомления интерфейса",
    CurrentValue = true,
    Callback = function(enabled)
        notificationsEnabled = enabled
        if enabled then
            LucidUI:Notify({ Title = "ATLAS", Message = "Уведомления включены.", Duration = 2 })
        end
    end,
})

Overview:CreateSlider({
    Name = "Длительность уведомления",
    Range = { 2, 6 },
    CurrentValue = 3,
    Increment = 1,
    Callback = function(value)
        notificationDuration = value
    end,
})

-- APPEARANCE
local AppearanceTab = Window:CreateTab({ Name = "Внешний вид", Icon = "settings" })
local Appearance = AppearanceTab:CreateSection({ Name = "Цветовая тема" })

Appearance:CreateTextDisplay({
    Title = "ПРЕДПОЧТЕНИЯ",
    Content = "Выбери сдержанную тему. Изменения применяются сразу ко всему окну.",
})

Appearance:CreateDropdown({
    Name = "Тема интерфейса",
    Options = { "Nord", "Midnight", "Ayu Mirage", "One Dark", "Dracula" },
    CurrentOption = "Nord",
    Callback = function(themeName)
        Window:SetTheme(themeName)
    end,
})

Appearance:CreateButton({
    Name = "Вернуть тему Nord",
    Callback = function()
        Window:SetTheme("Nord")
    end,
})

-- INFO
local InfoTab = Window:CreateTab({ Name = "Информация", Icon = "bell" })
local Info = InfoTab:CreateSection({ Name = "О проекте" })

Info:CreateTextDisplay({
    Title = "ATLAS UI",
    Content = "Версия интерфейса: 1.0\nБиблиотека: LucidUI\nПрофиль: Nord / минималистичный тёмный\n\nЭто демонстрационный интерфейс. Переключатели и ползунок управляют только самим меню и тестовыми уведомлениями.",
})

Info:CreateButton({
    Name = "Показать уведомление о готовности",
    Callback = function()
        LucidUI:Notify({
            Title = "ATLAS готов",
            Message = "Меню открыто и отвечает на касания.",
            Duration = 3,
        })
    end,
})

LucidUI:Notify({
    Title = "ATLAS",
    Message = "Интерфейс загружен.",
    Duration = 2,
})
