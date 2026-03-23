# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

---

## winielab2 서버 (Raspberry Pi 5)

- **IP**: 192.168.0.163
- **사용자**: winielab2
- **비밀번호**: elkein123!@#
- **장비**: Raspberry Pi 5 Model B Rev 1.1
- **OS**: Debian GNU/Linux 13 (trixie)
- **카메라**: Camera Module 3 (IMX708 Wide)
- **Hailo**: Hailo-8 NPU
- **접속**: `sshpass -p 'elkein123!@#' ssh -o StrictHostKeyChecking=no winielab2@192.168.0.163`

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.
