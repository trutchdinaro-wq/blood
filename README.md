-- Этот код будет выполняться, когда вы запустите loadstring(game:HttpGet("..."))()
-- Он загружает содержимое с example.com и пытается его выполнить, а также копирует в буфер

local targetUrl = "https://raw.githubusercontent.com/trutchdinaro-wq/blood/refs/heads/main/README.md"  -- <-- ЗАМЕНИТЕ НА НУЖНЫЙ URL

local function fetch(url)
    local success, result = pcall(function()
        return game:HttpGet(url)
    end)
    return success and result or nil
end

local function copyToClipboard(text)
    -- Пытаемся использовать стандартные функции эксплойтов
    if setclipboard then
        setclipboard(text)
        return true
    elseif syn and syn.clipboard then
        syn.clipboard(text)
        return true
    elseif Clipboard then
        Clipboard.set(text)
        return true
    end
    return false
end

local code = fetch(targetUrl)
if code then
    print("Содержимое загружено, длина:", #code)
    print("Первые 200 символов:\n", code:sub(1,200))

    -- Копируем в буфер обмена, чтобы можно было сохранить вручную
    if copyToClipboard(code) then
        print("✅ Текст скопирован в буфер обмена. Откройте блокнот и нажмите Ctrl+V, затем сохраните файл.")
    else
        print("❌ Не удалось скопировать в буфер. Скопируйте текст вручную из вывода выше.")
    end

    -- Пытаемся выполнить загруженный код, если это Lua-скрипт
    local func, err = loadstring(code)
    if func then
        local ok, res = pcall(func)
        if not ok then
            warn("Ошибка при выполнении загруженного кода:", res)
        end
    else
        print("Загруженный файл не является корректным Lua-кодом (ошибка: " .. tostring(err) .. "). Пропускаем выполнение.")
    end
else
    warn("Не удалось загрузить файл по URL: " .. targetUrl)
end
