# cadesplugin-crypto-api

Библиотека предоставляет API для работы c cadesplugin и Крипто Про

Форк библиотеки [cadesplugin-crypto-pro-api](https://github.com/smodean/cadesplugin-crypto-pro-api)

## Install

npm i cadesplugin-crypto-api

## API

### about()

Выводит информацию о верисии плагина и так далее

### getCertsList(fromContainer=true)

Получает массив валидных сертификатов. По умолчанию берет сертификаты с носителей ЭП, если значение флага **false**, то берет из локального хранилища пользователя.

### currentCadesCert(thumbprint, fromContainer = true)

Получает сертификат по thumbprint значению сертификата

### getCert(thumbprint, fromContainer = true)

Получает сертификат по thumbprint значению сертификата.
С помощью этой функции в сертификате доступны методы для парсинга информации о сертификате

### signBase64(thumbprint, base64, type, fromContainer = true)

Подписать строку в формате base64

### signXml(thumbprint, xml, cadescomXmlSignatureType, fromContainer = true)

Подписать строку в формате XML

## Custom certs format API

### friendlySubjectInfo()

Возвращает распаршенную информацию о строке subjectInfo

### friendlyIssuerInfo()

Возвращает распаршенную информацию о строке issuerInfo

### friendlyValidPeriod()

Возвращает распаршенную информацию об объекте validPeriod

### possibleInfo(subjectIssuer)

Функция формирует ключи и значения в зависимости от переданного параметра
Доступные параметры 'subjectInfo' и 'issuerInfo'

### friendlyDate(date)

Формирует дату от переданного параметра

### isValid()

Прозиводит проверку на валидность сертификата

## Usage

```js
import ccpa from 'cadesplugin-crypto-pro-api';

/**
 * @async
 * @function doCertsList
 * @param {Boolean} fromContainer флаг, определяющий источник сертификата. По умолчанию - с носителя
 * @description формирует массив сертификатов с оригинальными значениями
 */
async function doCertsList(fromContainer = true) {
  const certsApi = await ccpa;
  const certsList = await certsApi.getCertsList(fromContainer);

  return certsList;
}

/**
 * @async
 * @function doFriendlyCustomCertsList
 * @param {Boolean} fromContainer флаг, определяющий источник сертификата. По умолчанию - с носителя
 * @description формирует массив сертификатов с кастомными полями
 */
async function doFriendlyCustomCertsList(fromContainer = true) {
  const certsApi = await ccpa;
  const certsList = await certsApi.getCertsList(fromContainer);

  const friendlyCertsList = certsList.map(cert => {
    const friendlySubjectInfo = cert.friendlySubjectInfo();
    const friendlyIssuerInfo = cert.friendlyIssuerInfo();
    const friendlyValidPeriod = cert.friendlyValidPeriod();
    const {
      to: { ddmmyy, hhmmss }
    } = friendlyValidPeriod;

    return {
      subjectInfo: friendlySubjectInfo,
      issuerInfo: friendlyIssuerInfo,
      validPeriod: friendlyValidPeriod,
      thumbprint: cert.thumbprint,
      title: `${
        friendlySubjectInfo.filter(el => el.value === 'Владелец')[0].text
      }. Сертификат действителен до: ${ddmmyy} ${hhmmss}`
    };
  });
}
```

### License

MIT
