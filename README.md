# How To Create A rpm Repository

```
sudo dnf install rpm-build rpmdevtools
```


```
rpmdev-setuptree
```

Das erzeugt ein Verzeichnis wie:
```
~/rpmbuild/
├── BUILD
├── RPMS
├── SOURCES
├── SPECS
└── SRPMS

```


Platziere dein Go-Binary (z. B. meintool) und Lizenzdateien nach ~/rpmbuild/SOURCES/:
```
cp meintool ~/rpmbuild/SOURCES/
cp LICENSE ~/rpmbuild/SOURCES/

```


```
cp meintool ~/rpmbuild/SOURCES/
cp LICENSE ~/rpmbuild/SOURCES/

```

Erstelle eine Datei meintool.spec in ~/rpmbuild/SPECS/. Beispiel:
```
Name:           meintool
Version:        1.0.0
Release:        1%{?dist}
Summary:        Mein tolles CLI-Tool in Go

License:        MIT
URL:            https://github.com/deinuser/meintool
Source0:        meintool
Source1:        LICENSE

BuildArch:      x86_64
Requires:       bash  # falls nötig

%description
Ein nützliches Tool, geschrieben in Go.

%prep

%build

%install
mkdir -p %{buildroot}/usr/local/bin
install -m 0755 %{SOURCE0} %{buildroot}/usr/local/bin/meintool

%files
%license LICENSE
/usr/local/bin/meintool

%changelog
* Thu Jul 24 2025 Thomas Mundt <thomas@example.com> - 1.0.0-1
- Erste Version

```

Führe den Build aus:
```
rpmbuild -bb ~/rpmbuild/SPECS/meintool.spec

```

Das erzeugte RPM findest du dann unter:
```
~/rpmbuild/RPMS/x86_64/meintool-1.0.0-1.el9.x86_64.rpm

```



## RPM öffentlich bereitstellen


Option 1: RPM direkt über HTTP(S) bereitstellen

Hoste das .rpm-Paket auf einem Webserver (z. B. GitHub Pages, nginx, S3, usw.). Dann kann es so installiert werden:
```
dnf install https://example.com/path/to/meintool-1.0.0.rpm

```

Option 2: Eigene YUM/DNF-Repo-Datei bereitstellen
```
Falls du mehrere Pakete bereitstellen willst, kannst du dein eigenes Repository erzeugen (z. B. mit createrepo) und in /etc/yum.repos.d/meintool.repo eintragen.
```


```
Option 3: Paket in einen öffentlichen Repository-Dienst hochladen
copr.fedorainfracloud.org → Ähnlich wie GitHub Actions für RPM

Open Build Service (OBS) → Cross-distro-Builds (auch DEB)
```


Bonus: Automatisierung mit GitHub Actions
Falls dein Go-Binary aus dem Quellcode gebaut wird, kannst du RPMs auch automatisch mit GitHub Actions erstellen und auf GitHub Releases hochladen.

