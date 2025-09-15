# Fair Fix - Wymagania Biznesowe i Funkcjonalne

## 1. Cel Projektu

**Problem biznesowy:**
- Trudność w analizie kosztów części samochodowych z faktur dostawców
- Manualne przepisywanie danych z dokumentów (błędy, czas)
- Brak efektywnego porównywania cen między dostawcami
- Problemy z rozpoznawaniem pisma ręcznego na fakturach

**Rozwiązanie:**
System automatycznego skanowania i analizy faktur z częściami samochodowymi wykorzystujący AI do wyodrębniania danych i porównywania cen.

## 2. Stakeholders

### Główni użytkownicy:
- **Warsztaty samochodowe** - analiza kosztów części
- **Dystrybutorzy części** - monitoring konkurencji
- **Fleet managenrzy** - optymalizacja kosztów flotowych

### Użytkownicy końcowi:
- Mechanicy (upload dokumentów)
- Menedżerowie warsztatów (analiza raportów)
- Księgowość (weryfikacja faktur)

## 3. Wymagania Funkcjonalne - Faza 1 (MVP)

### 3.1 Upload Dokumentów
**FR-001: Obsługa formatów plików**
- **Wymaganie:** System musi przyjmować pliki PDF, JPG, PNG, TIFF
- **Priorytet:** Wysoki
- **Akceptowalne rozmiary:** do 10MB na plik
- **Rozdzielczość:** minimum 150 DPI dla skanów

**FR-002: Obsługa różnych źródeł**
- Skany faktur z drukarek
- Zdjęcia zrobione telefonem
- Dokumenty pisane ręcznie
- Faktury elektroniczne (PDF)

### 3.2 Przetwarzanie Dokumentów

**FR-003: Rozpoznawanie pozycji faktur**
- **Wymagane pola:**
  - Nazwa części/usługi
  - Ilość (sztuki)
  - Cena jednostkowa netto
  - Cena jednostkowa brutto
  - Wartość netto pozycji
  - Wartość brutto pozycji
- **Dokładność:** minimum 90% dla drukowanych dokumentów
- **Dokładność:** minimum 70% dla pisma ręcznego

**FR-004: Rozpoznawanie danych pojazdu**
- **Wymagane pola:**
  - Marka pojazdu
  - Model pojazdu
  - Typ silnika
  - Rocznik pojazdu
  - Numer VIN
- **Źródła:** nagłówek faktury, opis pozycji, dodatkowe pola

**FR-005: Walidacja danych**
- Kontrola formatów numerów części
- Weryfikacja poprawności cen (wartość = ilość × cena)
- Sprawdzenie sum częściowych i końcowych
- Flagowanie podejrzanych wyników

### 3.3 Prezentacja Wyników

**FR-006: Strukturalizacja danych wyjściowych**
```json
{
  "document_info": {
    "supplier": "string",
    "document_number": "string", 
    "date": "YYYY-MM-DD",
    "total_net": "number",
    "total_gross": "number"
  },
  "vehicle_info": {
    "brand": "string",
    "model": "string", 
    "engine": "string",
    "year": "number",
    "vin": "string"
  },
  "items": [{
    "name": "string",
    "part_number": "string",
    "quantity": "number",
    "unit_price_net": "number", 
    "unit_price_gross": "number",
    "total_net": "number",
    "total_gross": "number",
    "confidence": "number"
  }],
  "processing_info": {
    "confidence_score": "number",
    "processing_time": "number",
    "warnings": ["string"]
  }
}
```

**FR-007: Interface użytkownika**
- Drag & drop upload dokumentów
- Podgląd oryginalnego dokumentu
- Edytowalną tabelę wyodrębnionych danych
- Wskaźniki pewności rozpoznania
- Możliwość eksportu do CSV/Excel

## 4. Wymagania Niefunkcjonalne

### 4.1 Performance
- **NFR-001:** Przetwarzanie dokumentu < 30 sekund
- **NFR-002:** Obsługa równoczesnych uploadów (min. 5 użytkowników)
- **NFR-003:** Czas odpowiedzi API < 2 sekundy

### 4.2 Skalowalność
- **NFR-004:** Przygotowanie pod hosting na domenie publicznej
- **NFR-005:** Architektura umożliwiająca dodanie bazy danych
- **NFR-006:** Możliwość rozbudowy o porównywanie cen

### 4.3 Bezpieczeństwo
- **NFR-007:** Szyfrowanie przesyłanych dokumentów
- **NFR-008:** Brak przechowywania dokumentów po przetworzeniu
- **NFR-009:** Walidacja typów i rozmiarów plików

### 4.4 Dostępność
- **NFR-010:** Interface w języku polskim
- **NFR-011:** Responsywność na urządzeniach mobilnych
- **NFR-012:** Dostępność 99% (przyszłość)

## 5. Ograniczenia i Założenia

### 5.1 Ograniczenia techniczne
- Dependencja od Google Document AI API
- Limity API (1000 dokumentów/miesiąc w wersji darmowej)
- Wymaga połączenia internetowego

### 5.2 Założenia biznesowe  
- Użytkownicy mają dostęp do skanerów lub aparatów
- Dokumenty zawierają wymagane informacje
- Akceptowalna dokładność 70-90% w MVP

### 5.3 Ograniczenia MVP
- Brak porównywania cen (Faza 2)
- Brak bazy danych części (Faza 2)
- Brak systemu użytkowników (Faza 2)
- Brak persist dokumentów (tylko processing)

## 6. Kryteria Akceptacji MVP

**Definicja "Done" dla MVP:**
1. ✅ Użytkownik może uploadować dokument (PDF/obrazy)
2. 🔄 System przetwarza dokument przez Google Document AI
3. 🔄 System wyodrębnia pozycje faktur z 70%+ dokładnością
4. 🔄 System rozpoznaje dane pojazdu z 60%+ dokładnością  
5. 🔄 Wyniki prezentowane w przejrzysty sposób
6. 🔄 Możliwość eksportu wyników do CSV
7. 🔄 Interface działa na urządzeniach mobilnych

**Metryki sukcesu:**
- Czas przetwarzania < 30 sekund
- Dokładność rozpoznawania > 70%
- Zero błędów krytycznych w interfejsie
- Pozytywny feedback od 3 użytkowników testowych