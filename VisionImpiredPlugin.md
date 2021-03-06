        $(document).ready(function (e) {
            $("body").RuAdesSpecial();
        });


        (function ($) {
            /**
             * ades.ru - сайты и приложения
             *
             * плагин обеспечивает работу спецверсии сайта
             * - добавляет className к тегу Body
             * - сохраняет статус enabled or disabled в cookies
             * - загружает нужную версию по записи в cookies
             * - управляет текстом кнопки
             * остается только добавить стили для className
             * чтобы отображать контент как нужно
             */
            $.fn.RuAdesSpecial = function () {
                var $button = $("#specialButton"),
                    normalButtonText = "обычная версия",
                    specialButtonText = "версия для слабовидящих",

                    enabledStatus = "enabled",
                    disabledStatus = "disabled",
                    defaultStatus = disabledStatus,

                    cookieName = "ru_ades_special_enable",
                    className = "ru_ades_special",
                    _this = this,
                    curStatus;

                init();

                function init() {
                    // если плагин был назначен не BODY, то отсановим обработку
                    if ( _this.get( 0 ) !== $("body").get( 0 ) ){
                        console.log( 'Плагин должен быть подключен к тегу body!' );
                        return false;
                    }
                    //проверим куки
                    if (!navigator.cookieEnabled) {
                        console.log( 'Включите cookie для сохранения своего выбора версии!' );
                    }
                    //проверим статус, если нет, установим дефолтный
                    curStatus = getCookie(cookieName);
                    if ( curStatus === undefined ){
                        curStatus = defaultStatus;
                    }
                    processStatus(curStatus);
                }

                function processStatus(status) {
                    setPageClass(status);
                    setButtonText(status);
                }

                function setCookie(name, value, expires, path, domain, secure) {
                    document.cookie = name + "=" + escape(value) +
                        ((expires) ? "; expires=" + expires : "") +
                        ((path) ? "; path=" + path : "") +
                        ((domain) ? "; domain=" + domain : "") +
                        ((secure) ? "; secure" : "");
                }

                function getCookie(name) {
                    var matches = document.cookie.match(new RegExp(
                        "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
                    ));
                    return matches ? decodeURIComponent(matches[1]) : undefined;
                }

                /**
                 * возвращает год следующий за текущим
                 * @returns {string}
                 */
                function getNextYearDate() {
                    var date = new Date();
                    date.setFullYear(date.getFullYear() + 1);
                    return date.toUTCString();
                }

                function setButtonText(status) {
                    if (status === enabledStatus) {
                        $button.html(normalButtonText);
                    } else {
                        $button.html(specialButtonText);
                    }
                }

                function setPageClass(status) {
                    if (status === enabledStatus) {
                        _this.addClass(className);
                    } else {
                        _this.removeClass(className);
                    }
                }

                $button.on("click", function () {
                    var status = (_this.hasClass(className))? disabledStatus: enabledStatus;
                    setCookie(cookieName, status, getNextYearDate(), "/");
                    processStatus(status);
                });
            };
        })(jQuery);
